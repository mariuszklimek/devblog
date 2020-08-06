---
layout: post
title: "Debugging in Blazor WebAssembly - few observations"
date: 2020-08-06 09:31:03 +0100
permalink: /blazor/wasm/debugging
categories: csharp blazor-assembly debugging
---

One of the few things I try to learn in my free time is Blazor.

Recently I was playing with Blazor WebAssembly in my small pet project I don't think will have a future - I call it [IRMa](https://github.com/klimcio/Irma), for short ;-)

When trying to debug some problems I noticed one thing:

**Breakpoints do not work in Blazor WASM**

Which made me think whether I need to use Visual Studio (instead of VS Code, as it is simpler) for Blazor WebAssembly projects ;-)

After some research and trial and error I learned that it's not that bad - **you can use the browser's DevTools console window**.

All exceptions will be written in the console window, inluding the ones you throw yourself. With callstack and error message.

So I did one more test:

```c#
Console.WriteLine("Test");
```

And learned it works as well.

So... no breakpoints, but debugging the old school way works :-)

Now, back to learning ;-)