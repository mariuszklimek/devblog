---
layout: post
title:  "Top 6 things you need to know about discards in C#"
date:   2020-02-18 00:45:03 +0100
permalink: /csharp/discards/
categories: csharp discard
---

I love with people. Especially, when someone uses a new feature and I get a WTF moment that leads to learning something new. Imagine my discovery when reading changes in an ASP.NET Core MVC project and seeing this:

```c#
_ = services
```

Where is the declaration of the _ variable? I did not see it. I asked my rubber ducky. What is this? Why is it so badly named? I thought I will never find it using Google. Thank God [I did](https://docs.microsoft.com/en-us/dotnet/csharp/discards).

The following article can be found inside the linked article and this blog post should be considered a documentation of my learning process. Made possible by the wonderful people writing docs for C# :-)

## 1. It is a void for unwanted values

Discards are temporary, dummy variables, **intentionally unused** in application code. They do not have a value you can use later. You don't use them later. **Discards are there to reduce memory allocations**. Because they make the intent of your code clear, they enhance its readability and maintainability.

## 2. You can replace it with a variable scope wise

## 3. Use it to decompose a tuple
(_, _, area) = city.GetCityInformation(cityName);

## 4. Use it when checking parse results without the need of declaring new variables
 if (DateTime.TryParse(dateString, out _)) {
	Console.WriteLine($"'{dateString}': valid"); 
}

## 5. Use it for null checks
_ = myVariable ?? throw new ArgumentNullException("IT'S A NULL");

## 6. If you assign a Task to it, it will suppress any exceptions thrown inside

```c#
 _ = Task.Run(() => { 
	var iterations = 0; 
	for (int ctr = 0; ctr < int.MaxValue; ctr++) iterations++; 
	Console.WriteLine("Completed looping operation..."); 
	throw new InvalidOperationException(); 
}); 
```
