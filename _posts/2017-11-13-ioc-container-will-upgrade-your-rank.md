---
id: 93
title: IoC container will upgrade your rank!
date: 2017-11-13T20:53:14+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=93
permalink: /c-sharp/ioc-container-will-upgrade-your-rank/
dsq_thread_id:
  - "6281953301"
categories:
  - Dev Diary
tags:
  - 'c#'
  - Inversion of Control
  - structuremap
---
This post took some time to write. And I have to start with a confession. I didn't use an IoC container (IoC stands for Inversion of Control) in [my last example](http://blog.mariuszklimek.eu/adventures-in-c-sharp/automate-work-with-interface/) and I got scolded for it.

It's not that I never worked with IoC containers before. But little experience as I had I always had everything configured long before I joined the projects. After receiving this critique I decided that it is the best chance to finally make up the shortcomings and make this legacy code better :-)

I won't change the code from the last example much. As a matter of fact, only one section - **Using the validators** - will be changed.

I decided to use StructureMap to modify the examples.

* Declaring variables

Required NuGet package: **StructureMap**

First, we have to declare the IoC container:

```c#
private DI.Container _container;
```

I run into a reference problem and had to ensure that I point to StructureMap's Container, which led me to this using:

```c#
using DI = StructureMap;
```

And then we need to initialize it:

```c#
Constructor() {
   _container = new DI.Container(c => { c.AddRegistry<OurRegistry>(); });  
}
```

Next, we create the registry...

# Creating a registry

The registry is a class that inherits from the StructureMap.Registry.

It looks kind of like this:

```c#
using StructureMap;

namespace OurProject
{
  public class OurRegistry : Registry
  {
    public OurRegistry()
    {
      For<IValidateData>().Use<BirthDateValidator>();
      For<IValidateData>().Use<AddressValidator>();
      For<IValidateData>().Use<MinimumIncomeThisYearValidator>();

      For<IDataValidator>().Use<DataValidator>();
    }
  }
}
```

The IDataValidator is a new class responsible for launching all the validating rules. How is it different from the previous code?

```c#
namespace OurProject
{
  public class DataValidator : IDataValidator
  {
    private readonly IEnumerable<IValidateData> _validators;

    public DataValidator(IEnumerable<IValidateData> validators)
    {
      _validators = validators;
    }

    ...
  }
}
```

[Previously](http://blog.mariuszklimek.eu/adventures-in-c-sharp/automate-work-with-interface/), I used System.Reflection to get all the validation rules objects by their interfaces. Now, we just pass all of them as dependencies to the DataValidator. No additional logic is needed.

# Using the IoC container

With the container declared, every time, we need an object we can use this line:

```c#
var fileParser = _container.GetInstance<IToBeInsuredParser>();
```

You do not have to pass anything to the constructor. The Registry takes care of that.

# **Summary?**

Sorry if this post is too brief. It sure took a while to write it. Not to mention, that I have been playing a little with ASP.NET Core recently and learned that dependency injection is used there by default. It's part of the architecture.

I suppose I'll try to cover that next ;-)