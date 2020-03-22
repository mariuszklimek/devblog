---
layout: post
title:  "Top 6 things you need to know about discards in C#"
date:   2020-02-18 00:45:03 +0100
permalink: /csharp/discards/
categories: csharp discard
---

I love working on projects with people. Sometimes someone uses something new, while after that I encounter it, go WTF and sometimes even learn something. Imagine my discovery when I saw this:

```c#
_ = services
```

I did not see the definition. Where is it declared? I asked my rubber ducky. What is this? Why is it so badly named? I thought I will never find it using Google. Thank God [I did](https://docs.microsoft.com/en-us/dotnet/csharp/discards). 
All this can be found inside the linked article and this blog post should be considered a documentation of my learning process.

# 1. It is a void for unwanted values

Temporary, dummy variables, intentionally unused in application code. They do not have a value. 
Because there is only a single discard variable, and that variable may not even be allocated storage, **discards can reduce memory allocations**. Because they make the intent of your code clear, they enhance its readability and maintainability.
https://docs.microsoft.com/en-us/dotnet/csharp/discards
2. You can replace it with a variable scope wise
3. Use it to decompose a tuple
(_, _, area) = city.GetCityInformation(cityName);
4. Use it when checking parse results without the need of declaring new variables
 if (DateTime.TryParse(dateString, out _)) {
	Console.WriteLine($"'{dateString}': valid"); 
}
5. Use it for null checks
_ = myVariable ?? throw new ArgumentNullException("IT'S A NULL");
6. If you assign a Task to it, it will suppress any exceptions thrown inside

```c#
 _ = Task.Run(() => { 
	var iterations = 0; 
	for (int ctr = 0; ctr < int.MaxValue; ctr++) iterations++; 
	Console.WriteLine("Completed looping operation..."); 
	throw new InvalidOperationException(); 
}); 
```
