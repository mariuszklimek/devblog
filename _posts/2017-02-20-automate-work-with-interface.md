---
id: 78
title: How to use an interface to automate work!
date: 2017-02-20T20:40:04+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=78
permalink: /codeproject/automate-work-with-interface/
dsq_thread_id:
  - "5569653915"
categories:
  - Dev Diary
tags:
  - Activator
  - 'c#'
  - interface
  - system.reflection
---
Difficulty level: **Beginner** (if you understand the concept of an interface it should be easy)

Programming language: **C#**

Imagine you have a custom object, which requires some complex validation or data integration.

For example:

You work in a bank. You need to validate if a customer can take a credit. You have a big object with your customer's essential data. You also have multiple business rules that decide whether your customer can take the credit, or maybe even get a better credit offer.

How do you do it? I suppose you already moved your code to separate methods, or even classes. But you still execute the methods one after another:

```c#
ValidateBirthDate(myData);
ValidateAddress(myData);
ValidateMinimumIncomeThisYear(myData);
...
ValidateSomethingMore(myData);
```

If so, let me introduce you to a solution I learned some time ago at work and managed to implement it a few times. It's not very expensive and it may prove very convenient in further usage and in unit tests coverage.

What we are going to do is:

  1. write an interface to represent validation functions
  2. implement the interface in few separate classes
  3. use some simple code to get all the validators

# The interface

Let's keep it simple for the sake of the example :-)

```c#
public interface IValidateData
{
    List<string> Validate(MyCustomData data);
}
```

You might be wondering why I named the interface like that, instead of IDataValidator. I tend to experiment in my pet projects, so I let the "I" describe the object in a more readable fashion: "I Provide Services" instead of "IServiceProvider". Name it anyway you want :-)

The return type is a list of strings - in this case a list of error messages. If the object returns an empty list it means that there are no errors. This particular part was made in a little haste, so perhaps it would be better to replace it with something that is better in your case.

# The validator class

Now the easy part. You must move the code you use for validation to their separate classes. How? Just implement the interface :-)

```c#
public class BirthDateValidator : IValidateData
{
    public List<string> Validate(MyCustomData data)
    {
        ...
    }
}

public class AddressValidator : IValidateData { ... }

public class MinimumIncomeThisYearValidator : IValidateData { ... }

...

public class SomethingMoreValidator : IValidateData { ... }
```

Easy as that. How you want to name them, arrange them and all is up to you.

# Using the validators

[Use an IoC Container](http://blog.mariuszklimek.eu/adventures-in-c-sharp/ioc-container-will-upgrade-your-rank/). Ignore the rest ;-)

First, we need to get all the validators that implement our interface:

```c#
var validators = Assembly.GetExecutingAssembly()
                         .GetTypes()
                         .Where(mytype => mytype.GetInterfaces().Contains(typeof(IValidateData)))
                         .Select(x => (IValidateData) Activator.CreateInstance(x))
                         .ToList();
```

We basically get all types that implement the IValidateData interface, initialize them and save them to a generic list of IValidateData objects.

If you want to use this solution for different purposes in your project, I recommend you name them very specifically.

Given we already have an object waiting to be validated the usage looks something like that:

```c#
MyCustomData package;
...
// Let's agree this is the place when we initialize the package variable
...
List<string> validationMessages;
foreach (var validator in validators)
{
    validationMessages.AddRange(validator.Validate(package));
}
```

# **What I gained from implementing validation like this?**

* Write the usage once, then only implement the interface in new classes.
* Unit tests are easier to write, since each validation rule is contained in a separate class.
* More robust code - we don't need to worry about the list of validation methods to be called.

So, how do you like it?