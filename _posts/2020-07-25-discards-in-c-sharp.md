---
layout: post
title: "6 things you need to know about discards in C#"
date: 2020-07-25 23:5:03 +0100
permalink: /csharp/discards/
categories: csharp discard
---

Quite recently a colleague of mine did something like this in the codebase:

```c#
_ = services
```

And since I couldn't find a definition of _ I asked my rubber ducky. What is this? Why is it so badly named? I thought I will never find it using Google. Thank God [I did](https://docs.microsoft.com/en-us/dotnet/csharp/discards). 

All the information I present here can be found inside the linked article above and this blog post should be considered a documentation of my learning process.

So here's the beginner's guide to discards in 6 fancy points :-)

# 1. It is a void for unwanted values

It's called a discard and it can be defined as:
- temporary
- dummy variables
- that do not have a value
- **that reduce memory allocations** (a single discard variable that may not even be allocated storage) 
- and it's intentionally unused in application code
- and because they make the intent of your code clear, they enhance its readability and maintainability.

[Source](https://docs.microsoft.com/en-us/dotnet/csharp/discards)

# 2. You can replace it with a variable scope wise
# 3. Use it to decompose a tuple

```c#
(_, _, area) = city.GetCityInformation(cityName);
```

# 4. **My personal favorite**: Use it when checking parsing results without the need of declaring new variables

```c#
if (DateTime.TryParse(dateString, out _)) {
	Console.WriteLine($"'{dateString}': valid"); 
}
```

# 5. Use it for null checks

```c#
_ = myVariable ?? throw new ArgumentNullException("IT'S A NULL");
```

# 6. If you assign a Task to it, it will suppress any exceptions thrown inside

```c#
 _ = Task.Run(() => { 
	var iterations = 0; 
	for (int ctr = 0; ctr < int.MaxValue; ctr++) iterations++; 
	Console.WriteLine("Completed looping operation..."); 
	throw new InvalidOperationException(); 
}); 
```

# Where to go next?

As for me, I guess I will be returning to tuples. I hate tuples. They're unreadable and often not helping. But I might change my mind.

Also, perhaps I will use discards for null checks.

# End thoughts

This time I tried to keep everything concise. Instead of paragraphs with explanations I went to advanced text formatting. Headers and code snippets can be self-explaining and I will not waste your time.