---
layout: post
title: "Obvious tips for F# Beginners"
date: 2020-05-20 22:45:03 +0100
permalink: /fsharp/beginner-tips/
categories: fsharp net-core tutorial beginner
---

I started to learn F#. 

It's funny to start learning something that different after years of programming in C#. The syntax itself seems so unintuitive, although design choices make sense. 

And since I'm doing this in .NET Core 3.1 it's a bit different from many tutorials I managed to get.

In this post I will share some tips that made my learning ongoing ;-)

## My HelloWorld application

Using **VS Code** and the **command line** I created a project using command line:
```
dotnet new console -lang F# -o SimpleExample
```
And added some code:
```fsharp
open System

[<EntryPoint>]
let main argv =
    let square x = x * x
    let squared = List.map square [1;2;3]
    0 // return an integer exit code
```
And run it using:
```
dotnet run
```

Let me share you few tips on how to make learning F# in .NET Core less painful ;-)

## Printfn cheatsheet




## Spaces at the end matter.

Since running this code with `dotnet run` did not view any results (I am using VSCode) I have written a new line before 0:

```fsharp
    printfn "%A" squared 
```

But running the code now returned an error:

```
error FS0597: Successive arguments should be separated by spaces or tupled, and arguments involving function or method applications should be parenthesized
```

Pointing to the beginning of the word "squared".
Guess what... There was a single space after squared. Removing it fixed the bug.

Now, let's get back to learning!