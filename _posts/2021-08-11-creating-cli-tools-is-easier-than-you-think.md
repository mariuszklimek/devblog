---
layout: post
title: "Creating CLI tools is easier than you think"
date: 2021-05-01 09:31:03 +0100
permalink: /cli/getting-started-params
categories: cli C#
---

During my work on my [Deprivatizer](https://github.com/klimcio/deprivatizer) script I wanted to learn a little bit about making CLI tools in C#. I had to write some really weird things in Google, because I learned about many libraries one can download as NuGet packages and then configure them and then fail to implement them at first.

So I tried something different.

I built a simple console application

```c#
using System;

namespace Deprivatizer.Cmd
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

Yes, I did not change a thing.
I put a break point at the start.
Went to the projects properties, and added this line

    smth.xml -super "this is a test"

in **Application arguments**.

When I run the application I noticed, that I got 3 arguments. It is actually parsed as command line parameters. Not simply split by spaces, but taken quotes when interpreting strings.

Srsly, implementing your own CLI will be easier than you think.