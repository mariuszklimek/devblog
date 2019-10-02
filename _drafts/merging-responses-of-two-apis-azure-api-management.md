---
layout: post
title:  "Calling two APIs at once and merging their results in Azure API Management"
date:   2019-10-02 00:45:03 +0100
permalink: /azure/api-management/two-apis-at-once
categories: azure azure-api-management azure-policies
---

Today's post will be about work achievements. In a project I'm involved in I was suppose to investigate how to solve a certain problem in Azure Api Management. Given the fact that I'm less and less hyped by Azure I tackled this challenge. Even if my experience isn't perhaps the best there is.

What was the problem, you ask? Let's say we have an API deployed to two separate environments. Let's say we need to call them both at once and do something with the data (mind that the structure is basically the same). How to approach this? My current solution is based on mocks, let me describe it to you.

# My little sandbox

I created two blank APIs that serve me as mock providers. Each resembles a different environment, both returning a mock response with a structure like this:

```json
{"env1": [{}, {}]}
```

What kind of data is inside those objects is without meaning at this point. What is important, that the response body has an array of custom objects returned.

What I want is for Azure API Management to help me merging two responses into one, in the end looking like that:

```json
{
  "success": true,
  "env1": [{}, {}],
  "env2": [{}, {}, {}]
}
```

# A proxy API

To achieve this I created another blank API in which there's a single operation.

Every policy here was added in the inbound section. I left the base at the beginning.

```xml
<inbound>
    <base />
```

First I declared a variable for every API call, that will store responses for me:

```xml
<set-variable name="env1" value="" />
```

Then I called the two APIs and stored the result:

```xml
<send-request mode="new" response-variable-name="env1" timeout="20" ignore-error="false">
  <set-url>https://this-api-of-mine.azure-api.net/env1api/data</set-url>
  <set-method>GET</set-method>
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
</send-request>
```

The `response-variable-name` attribute must point to the previously created variable.

Next I create another variable (it is not really needed, I added it to have more readability):

```xml
<set-variable name="env1Body" value="@(((IResponse)context.Variables["env1"]).Body.As<string>())" />
```

As we declared 

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
      <value>application/json</value>
  </set-header>
  <set-body template="liquid">
  {
      "success": true,
      "env1": {{ context.Variables["env1Body"] }},
      "env2": {{ context.Variables["env2Body"] }}
  }
  </set-body>
</return-response>
```



```xml
<set-variable name="env1Body" value="@(((IResponse)context.Variables["env1"]).Body.As<JObject>()["env1"].ToString())" />
```