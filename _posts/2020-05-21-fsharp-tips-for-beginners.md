---
layout: post
title: "Simple and helpful tips for F# Beginners"
date: 2020-05-21 22:45:03 +0100
permalink: /fsharp/beginner-tips/
categories: fsharp net-core tutorial beginner
---

I wanted to learn functional programming, and since I'm a .NET guy, so I started learning F#.

It's funny to start learning something that different after years of programming in C#. The syntax itself seems so unintuitive, although design choices make sense. 

And since I'm doing this in .NET Core 3.1 it's a bit different from many tutorials I managed to get. Like, no fsc (F# compiler) available in the commandline, and so on.

Therefore I decided to share my steps in the form of tips. If someone has problems with learning F# perhaps they can learned on my mistakes and save some time. By no means this post is to be considered different than a simple developer diary from learning something everyone else already does ;-)

## My HelloWorld application

Using **VS Code** and the **command line** I created a project. First I executed this in the command line:
```
dotnet new console -lang F# -o SimpleExample
```
Then I added some code:
```fsharp
open System

[<EntryPoint>]
let main argv =
    let square x = x * x
    let squared = List.map square [1;2;3;5;7]
    0 // return an integer exit code
```
And run it:
```
dotnet run
```

You'll never guess... I never saw an output ;-)

## Printfn cheatsheet

Since running this code with `dotnet run` did not view any results (I am using VSCode) I had to write a printfn to see what is happening:
```fsharp
open System

[<EntryPoint>]
let main argv =
    let square x = x * x
    let squared = List.map square [1;2;3;5;7]
    printfn "%s" squared
    0 // return an integer exit code
```
But this resulted in an error.
```
This expression was expected to have type 'string' but here has type 'int list'
```
The `%s` is ofcourse the simplest form of transforming value into message you can find in any tutorials. But, unlike C#, values are not converted to string by default. F# likes to brag that thanks to it's powerful type inference system you almost never have to specify the type of an object. Well, you learn the **almost never** quite fast to be honest ;-)

In this case you just had the use this:
```fsharp
printfn "%A" squared 
```
This basically converts the entire array into strings - showing it's values like this:
```
[1; 4; 9; 25; 49]
```
But sometimes you might have different values to view in the output window. So I prepared a small cheatsheet of types I found and how to represent them:
|Format|Type|
|--|--|
|%s|string|
|%i|integer|
|%u|unsigned integer|
|%f|float|
|%b|boolean|
|%O|other objects|
|%o|octal|
|%x|lowercase hex|
|%X|uppercase hex|
|%A|native F# types|
What are native F# types? Non-primitive types. Like: arrays, tuples, records and union types. Probably much more - to be learned.

## Spaces at the end matter.

Having the correct code resulted in a new error:
```
error FS0597: Successive arguments should be separated by spaces or tupled, and arguments involving function or method applications should be parenthesized
```
Pointing to the beginning of the word "squared".
Guess what... There was a single space after squared. Removing it fixed the bug. WHITESPACES MATTER, PEOPLE! ;-)

Now, let's get back to learning!