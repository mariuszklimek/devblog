---
layout: post
title: "File Order in F# - the most annoying thing for a beginner?"
date: 2020-06-04 22:45:03 +0100
permalink: /fsharp/file-order-in-fsharp/
categories: fsharp net-core tutorial beginner
---

Back to learning F#. Recently, I learned something that is most probably the most annoying thing so far. It looks very discouraging seeing tutorials where people are reordering files in Visual Studio just to make everything compile.

I had to see it with my own eyes :-)

Using **VSCode** and **.NET Core 3.1** I created a small project
```
dotnet new console -lang F# -o "05 Modules"
```
I added a new file I called `Printer.fs`
```f#
module Printer

let printString string =
    printfn "%s" string
```
Then, I modified `Program.fs`
```f#
open Printer

[<EntryPoint>]
let main argv =
    printString "1"
    0 // return an integer exit code
```
And then I run using `dotnet run`.

The first thing I learned when adding a new file is that you must declare modules for F# files. The error message says, something more:

```
error FS0222: Files in libraries or multiple-file applications must begin with a namespace or module declaration, e.g. 'namespace SomeNamespace.SubNamespace' or 'module SomeNamespace.SomeModule'. Only the last source file of an application may omit such a declaration.
```

Which means that `Program.fs` does not need a module. Or does it?

When I added the new file into `fsproj`

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <RootNamespace>_05_Modules</RootNamespace>
  </PropertyGroup>

  <ItemGroup>
    <Compile Include="Program.fs" />
    <Compile Include="Printer.fs" />
  </ItemGroup>

</Project>
```

And I must confess, I added the last file `Printer.fs` manually. At the end, where else? ;-)

Depending on what is going on in the project, I got two different errors. At first I got:
```
error FS0222: Files in libraries or multiple-file applications must begin with a namespace or module declaration, e.g. 'namespace SomeNamespace.SubNamespace' or 'module SomeNamespace.SomeModule'. Only the last source file of an application may omit such a declaration.
```
At first I tried to add `module Program` inside `Program.fs`, I thought it has given me new errors, therefore progress (see the end of the post, as this was not a solution):
```
error FS0039: The namespace or module 'Printer' is not defined. Maybe you want one of the following:↔   Printf↔   PrintfModule

error FS0039: The value or constructor 'printArray' is not defined. Maybe you want one of the following:↔   printf↔   Printf↔   printfn↔   PrintfFormat
```
What else can be missing? The file is mentioned in fsproj, it has a module, it is referenced by `Program.fs`.

Well, here's the annoying frustrating part: file order. 

When I switched the file order in `fsproj` to
```xml
  <ItemGroup>
    <Compile Include="Printer.fs" />
    <Compile Include="Program.fs" />
  </ItemGroup>
```
It compiled and run.

Yes, that is a language feature :-)

PS. After making that program compile I removed `module Program` from `Program.fs` and it still worked. As the error message said: Program does not need to be in a module.