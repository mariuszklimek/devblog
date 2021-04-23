---
layout: post
title: "Not another blog engine - introduction"
date: 2021-04-22 09:31:03 +0100
permalink: /blazor/wasm/for-blogging
categories: csharp blazor-assembly blogging filesystem
---

# Story time
I had a blog running on Wordpress for a long time and it sucked.

- it required a server (costs, costs, costs)
- it used a SQL database for storing posts
- if you wanted an extension, you either had to find a free one (good luck) and wish it worked according to description, buy it, or implement it yourself in PHP (which by itself is discouraging)

Then, maybe a year ago, a friend at work introduced me to Jekyll. And the more I learned, the more I loved it:

- why have a database, when file system is the best database!
- why spend money on a server when I can host on GitHub Pages!
- why drown in PHP when I can... well, float above the water with Ruby (it's still better, right?)

Well, it appears not really. I still lacked something. Sure, Jekyll is still easier to extend than Wordpress, but it still required me to actually learn a new language. A very different language.

Then I learned about Blazor, did some tutorials and I thought... THIS IS IT! Given the fact how cool Blazor WebAssembly is, I might actually make a blog in it to learn it!

# What I want to achieve
1. A good **alternative to Jekyll** for people who don't want to learn/use Ruby, but prefer C#
2. Copy-Paste migration from Jekyll
3. Easy to customize
4. Easy to configure
5. Easy to install in your own .NET Core project

# What I did so far
Well, I started with [crying on Twitter](https://twitter.com/MauriceKlimek/status/1384426084847038464) that the default template is too large and immediately get [an answer](https://twitter.com/De_Boeck_Bart/status/1384429423357739011).

And this way I started using the [Blazor Minimal Project Template](https://marketplace.visualstudio.com/items?itemName=GregTrevellick.BlazorMinimalProjectTemplate) by [Greg Trevellick](https://marketplace.visualstudio.com/publishers/GregTrevellick). That's it for now. Plan is to implement reading blog posts and turn markdown into html next.

I'm storing it on [GitHub](https://github.com/klimcio/BlazingBlogEngine) for everyone to see.

# Special thanks
This post would not be created (this fast) if it wasn't for [Andrzej Krzywda](https://twitter.com/andrzejkrzywda) who reignited my spark for blogging, again. Who knows, it might last longer than last time :-)