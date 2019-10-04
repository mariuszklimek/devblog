---
layout: post
title:  "Calling two APIs at the same time and merging their results in Azure API Management"
date:   2019-10-05 00:00:03 +0100
permalink: /azure/api-management/two-apis-at-once
categories: azure azure-api-management azure-policies
---

Today's post will be about achievements at work :-) In a project I'm involved in I was suppose to investigate Azure API Management, with a certain problem to solve to begin with.

What was the problem, you ask?

Let's say...

- we have an API deployed to two separate environments
- we need to call them both at the same time and return them in one response

There was this idea to just create an API for that, but this scenario will happen more often and we need something more comfortable than writing an API action everytime we need to merge some results.

Let me describe how I approached this issue.

## My little sandbox

For testing purposes, I created two blank APIs that serve me as mock providers. They will soon be replaced with the real deal. Each resembles a different environment, both returning a mock response with a structure like this:

```json
{"env1": [{}, {}]}
```

What kind of data is inside those objects is without meaning at this point. What is important, is that the response body has an array of custom objects returned.

What I want is for Azure API Management to help me merging two responses into one, in the end looking like that:

```json
{
  "success": true,
  "env1": [{}, {}],
  "env2": [{}, {}, {}]
}
```

## A proxy API

To achieve this I created another blank API with a single operation.

Inside of it are the policies required to get the job done.

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

Next I create another variable (it is not really needed, I added it to increase readability):

```xml
<set-variable name="env1Body" value="@(((IResponse)context.Variables["env1"]).Body.As<JObject>()["env1"].ToString())" />
```

Casting the body to a JObject with `As<JObject>()` allows us to traverse the JSON quicker, closer to a JavaScript syntax. After we get our list of objects we convert it to string again. Thanks to this, it remains a proper JSON.

One thing that remains is composing the response:

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

My first, and probably not the last venture into the world of Azure API Management :-)
