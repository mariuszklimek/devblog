---
id: 146
title: Logging into Azure from Powershell - or my beginner's steps to master Azure
date: 2018-07-14T22:37:36+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=146
permalink: /azure/logging-into-azure-from-powershell/
categories:
  - Dev Diary
tags:
  - Azure
  - Powershell
---
# Storytime

I've been working with Azure recently. Long enough to come to some weird noobie conclusions, like Azure seems to have been designed by former SharePoint developers ;-) It's very overwhelming, that's for sure, but once I got something to do, which isn't writing generic code for an Azure Function, I decided to write it down, just in case I will need it later and manage to forget it ;-)

First thing I realized is that deploying your solution on Azure can be a pain. After a week or so, I learned that stuff gets easier when introduced to Powershell. Even better, I can launch it on my local computer, and not through Azure Portal (which is very intimidating for me). It's easy and makes an impression, you'll see.

## TL;DR

  1. Logging into Azure from Powershell.

## Prerequisites

* An Azure subscription to which you log in :-)

## The main course

Basically, you can launch a Powershell script locally, that affects your Azure resources. Of course, you need to log in first.

To do that enter:

```ps1
Login-AzureRMAccount
```

A popup will appear asking you for your account name and password. And some other form of authentication (I got a code in an SMS). And... that's it...

You will something like this:

```ps1
Account          : <your-email>
SubscriptionName : <Whatever your subscription name is>
SubscriptionId   : <the subscription guid>
TenantId         : <the tenant's guid>
Environment      : <environment name>
```

**If you work with many subscriptions you might want to check if you logged to the correct one, because that's not necessarily the default one.** At my first chance... it wasn't. So I had to log out

```ps1
Logout-AzureRmAccount
```

And then repeated the login operation, but using a new parameter

```ps1
Login-AzureRMAccount -SubscriptionId blah-blah-blah
```

## Epilogue

I don't think my journeys in Azure land will be common. I prefer to work in some instant gratification areas - front end, unit tests and so on. My current work in Azure was mostly about "click to config" events, which isn't very fascinating ;-)