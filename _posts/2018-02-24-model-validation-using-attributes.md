---
id: 129
title: Model validation using attributes
date: 2018-02-24T22:13:59+00:00
author: Mariusz Klimek
layout: post
permalink: /c-sharp/model-validation-using-attributes/
dsq_thread_id:
  - "6502796231"
categories:
  - Dev Diary
tags:
  - asp.net mvc
  - attributes
  - 'c#'
  - validation
---
Model validation is one of those things you will never escape. It can haunt you, it can enlarge your controller's codebase uncontrollably, it can finally piss you off. But it can be done very easily. How? Let me show you that step by step.

# A basic scenario of model validation

Let's talk about a simple controller. We'll call it CustomerController and it will have only one form. A typical form. We generate it by a simple Index action:

```c#
public IActionResult Index()
{
    return View(new CustomerModel());
}
```

This action takes us to a view:

```c#
@model AspNetCoreMvc.Models.CustomerModel
<h2>@ViewBag.Message</h2>
@using (Html.BeginForm("Index", "Customer", FormMethod.Post)) {
    <div>
        @Html.LabelFor(m => m.Name)
        @Html.TextBoxFor(m => m.Name)
        @Html.ValidationMessageFor(m => m.Name)
    </div>
    <div>
        @Html.LabelFor(m => m.BirthDate)
        @Html.TextBoxFor(m => m.BirthDate)
        @Html.ValidationMessageFor(m => m.BirthDate)
    </div>
    <div>
    <button type="submit">Submit</button>
</div>
}
```

Two simple fields with validation messages. When we click Submit we are redirected to the post action of the same name:

```c#
[HttpPost]
public IActionResult Index(CustomerModel model)
{
    if (!ModelState.IsValid) {
        return View(model);
    }

    ViewBag.Message = "YAY!";

    return View(new CustomerModel());
}
```

Here if the ModelState is valid we will view to message "YAY!" on a clean form. If not it will return the form with the given model.

Do you get this feeling that right now there is no model validation here? Of course, ModelState is checked if it's valid, but how does he know what to check? If we'd launch the form and just press Submit with empty values, we'd see "YAY!" on screen. So no validation is in place. Let's look at the model:

```c#
using System;

namespace AspNetCoreMvc.Models
{
    public class CustomerModel
    {
        public string Name { get; set; }

        public DateTime? BirthDate { get; set; }
    }
}
```

It's simple, isn't it?

## Let's add some basic validation - repetition

Now we will modify the model and add a Required attribute.

```c#
using System;
using System.ComponentModel.DataAnnotations;

namespace AspNetCoreMvc.Models
{
    public class CustomerModel
    {
        [Required]
        public string Name { get; set; }

        public DateTime? BirthDate { get; set; }
    }
}
```

Now, if we launch the form, won't fill in any values and click Submit - we will get an error message: "**The Name field is required**" viewed by the ValidationMessage we set in the view.

So these are the very basics of model validation:

  1. We set basic validations by assigning attributes to the model's properties.
  2. We use ModelState.IsValid in the controller's actions to check if the fields are correctly filled.
  3. We display error messages by ValidationMessageFor helper.

Now, I will show you how to maintain everything that simple :-)

# Adding custom validations - how we all did it at some point

Let's add some custom validation.

For example, we want to ensure that none of our customers was born in the future.

First, I will show you the way I used for a long time. It's a way I don't recommend to you given what I've learned, but I suppose it is how many people do that (and therefore it will look somehow familiar).

```c#
[HttpPost]
public IActionResult Index(CustomerModel model)
{
    if (model.BirthDate > DateTime.Now)
    {
        ModelState.AddModelError("BirthDate", "We do not support customers from the future!");
    }

    if (!ModelState.IsValid)
    {
        return View(model);
    }

    ViewBag.Message = "YAY!";

    return View(new CustomerModel());
}
```

Seems easy, right? Although it has some disadvantages, for example:

* with every new validation, your controller's code gets more and more obese :-)
* when you want to return a validation message you need to remember the property name. And remember it every time the property's name changes

# **And now let's use the elegant ValidationAttribute way!**

So a few days ago I learned that there's a better a way. Since .NET 3.5 you can make your own validation possible by attributes. As you can validate strings as proper Emails or any fields by the fact, that they're required, you can also write your own validations. You use it by creating a new attribute that inherits from [ValidationAttribute](https://msdn.microsoft.com/en-GB/library/system.componentmodel.dataannotations.validationattribute(v=vs.110).aspx). It's the same attribute that is inherited in the RequiredAttribute class. And it influences the usage of ModelState.IsValid. How to write one? Just add a new attribute class that inherits from System.ComponentModel.DataAnnotations.ValidationAttribute.

When we will finish with it, here's how everything will change. Our controller action will look simpler, like in the beginning:

```c#
[HttpPost]
public IActionResult Index(CustomerModel model) {
    if (!ModelState.IsValid) {
        return View(model);
    }
    ViewBag.Message = "YAY!";
    return View(new CustomerModel());
}
```

Our model will have one attribute more:

```c#
public class CustomerModel
{
    [Required]
    public string Name { get; set; }

    [NoCustomersFromTheFuture]
    public DateTime? BirthDate { get; set; }
}
```

And all the logic will be hidden in the new attribute class:

```c#
public class NoCustomersFromTheFutureAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        var convertedDate = Convert.ToDateTime(value);

        if (convertedDate > DateTime.Now) {
            return new ValidationResult("We do not support customers from the future!");
        }

        return null;
    }
}
```

Looks similar to our validation code, doesn't it?

The only thing you could consider a disadvantage is that you have to convert your data from an object. But what about the benefits?

* Controllers have even less logic. You can go back to making your controllers cleaner.
* You don't have to think about modifying ModelState error list. You don't have to remember what field were you validating and remembering that in case you decide to change its name. The ValidationAttribute does that for you!
* You have reusable validation!

I found that it made my code better. Hope it'll help you improve yours :-)