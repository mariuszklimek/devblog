---
layout: post
title:  "Querying Application Insights - Public API and Azure API"
date:   2019-10-28 00:00:03 +0100
permalink: /azure/app-insights/api/querying
categories: azure azure-app-insights azure-active-directory c-sharp
---

Solving problems when working with an Azure resource is probably the perfect way to write blog posts. Lots of stuff to cover and assuming you finish what you started you will end up with something better than the Azure documentation ;-)

I use Application Insights on a daily basis at work, but when it came to monitoring it's dashboard capabilities are absurdly limited. Let's limit our sorrow to the fact that in order to render any kind of chart or list and pin it to a dashboard you need to have the data first. Want to create some kind of chart that will show an error that never occured but you know what will happen if it does? Well... You can't. If your query does not return data you cannot pin it to a custom dashboard. Weird, there I was thinking that in monitoring you should prepare for things that have not appeared so far :/

In the end we made our own monitoring dashboard.

In this post I will share how we've queried Application Insight using C#. First, using the Public API. Then, using Azure API.

## Table of Contents

1. Querying AppInsights step by step
2. Preparing an URL for Public API
3. Calling the Public API
4. Deserializing the result we've got
5. Preparing an URL for Azure API
6. Authenticating using Azure Active Directory
7. Calling the API

### Querying AppInsights step by step

For every approach we will do three steps:

1. Preparing an URL
2. Calling the API
3. Deserializing the result we've got.

### Preparing an URL for Public API

Based on [AppInsights documentation](https://dev.applicationinsights.io/documentation/Overview/URL-formats) in order to use the Public API we need to call this URL:

```text
https://api.applicationinsights.io/v1/apps/{appId}/query?query={query}
```

Inside it, we have two parameters:

- appId - the id of your AppInsights, you can learn [here](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID) how to get it,
- query - this is where you put your query

### Calling the Public API

Calling the API is your typical call by HttpClient:

```c#
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;

...

using (var client = new HttpClient())
{
   client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
   client.DefaultRequestHeaders.Add("x-api-key", apiKey);
   var response = client.GetAsync(requestUrl).GetAwaiter().GetResult();
   if (response.IsSuccessStatusCode)
   {
      return response.Content.ReadAsStringAsync().GetAwaiter().GetResult();
   }
}
```

The only parameter that requires a more detailed description is apiKey. You can learn [here](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID) on how to get this value.

### Deserializing the result we've got

This part is shared by both approaches. The json parameter is the string value returned by previous code.

```c#
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

...

private JToken DeserializeAppInsightsJson(string json)
{
   if (!IsJson(json))
   {
      return null;
   }
   var deserialized = JsonConvert.DeserializeObject<JToken>(json);
   var results = deserialized.First.First.First.Last.First;
   return results;
}

private bool IsJson(string input)
{
   input = input.Trim();
   var isJavaScriptObject = input.StartsWith("{") && input.EndsWith("}");
   var isJavaScriptArray = input.StartsWith("[") && input.EndsWith("]");
   return isJavaScriptObject || isJavaScriptArray;
}

```

The only part of this snippet that catches attention is the ugly part seen as `deserialized.First.First.First.Last.First`. I had two approaches to choose from here. Either create a set of classes that would resemble the structure of the returned JSON of the App Insights API... Or just query the JToken and get the part I really need - which resulted in the somehow ugly, yet quick to execute code. Since the structure is very much stable it would be a great idea to actually dive deeper into this structure and explain what is what. Some day :-)

This should work just fine. The only problem (and it is the problem that made us change it) is the [quota set for the application](https://dev.applicationinsights.io/documentation/Authorization/Rate-limits), which we reached quite fast. So we were forced to move to Azure API.

### Preparing an URL for Azure API

The link for Azure API requires the developer to provide more information:

```text
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/microsoft.insights/components/{componentName}/api/query?api-version={apiVersion}&query={query}
```

Let's talk about the parameters:

- subscriptionId - your subscription's id (just in case you were wondering ;-) )
- resourceGroupName - the name (not id) of the resource group your AppInsights instance is in
- componentName - the name (not id) of you AppInsights instance
- apiVersion - I suggest you keep the same API version I used - `2014-12-01-preview` - although you can test [other versions](https://dev.applicationinsights.io/documentation/API-Version), at your own risk ;-)
- query - as usual, it is your query

### Authenticating using Azure Active Directory

In order to authenticate the user I [created an app in Azure Active Directory](https://dev.applicationinsights.io/documentation/Authorization/AAD-Application-Setup). Although, [as I learned on Stack Overflow](https://stackoverflow.com/a/58335616/1693915), I could omitt this part. I haven't test it, but you might be interested in alternative options.

[The documentation for querying using AAD](https://dev.applicationinsights.io/documentation/Authorization/AAD-OAuth2-Flows) presents a solution that requires a RedirectUri. That wasn't exactly helpful, as I don't query via a user, and since all the calls are made with AJAX calls I couldn't just use a RedirectUri. With a little help from a friend I managed to find a better way to get the access token:

```c#
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Microsoft.Identity.Client;

...

public async Task<AuthenticationHeaderValue> GetAuthenticationHeaderValueAsync(string clientId, string clientSecret, string tenant)
{
   var clientCredentials = new ClientCredential(clientSecret);
   var app = new ConfidentialClientApplication(clientId, $"https://login.microsoftonline.com/{tenant}", "https://daemon", clientCredentials, null, new TokenCache());
   string[] scopes = {"https://management.azure.com/.default"};
   var result = await app.AcquireTokenForClientAsync(scopes).ConfigureAwait(false);
   return new AuthenticationHeaderValue("bearer", result.AccessToken);
}
```

- clientId - is basically your applicationId
- clientSecret - [is what you get when registered your app in AAD](https://dev.applicationinsights.io/documentation/Authorization/AAD-Application-Setup)
- Tenant - your company's tenant id.

### Calling the API

And the last part:

```c#
using (var client = new HttpClient())
{
client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

var response = client.GetAsync(url).GetAwaiter().GetResult();

return response.Content.ReadAsStringAsync().GetAwaiter().GetResult();
```

As you probably noticed, it looks almost the same as calling the Public API. Ofcourse you have to pass the accessToken you received from the last step. And ofcourse you will use the same methods for deserializing JSON that I described earlier in this text.

Hope it helps you :-)
