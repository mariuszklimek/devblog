---
layout: post
title: "Introducing my new little pet project - file deprivatizer"
date: 2021-08-10 21:05:03 +0100
permalink: /pet-projects/deprivatizer
categories: c# console-application xml
---

Quite recently, I was asked to do one script. A script that would parse XML files and convert them into another. Piece of cake, I thought.

I created a GitHub repository for it.
I started writing it.
I started doing Unit Tests... and immediately stopped.

Data wasn't the problem - I got sample files.
Problem was they were full of sensitive business data. 

I couldn't just update them in a repository and make them public!

Not to mention, these files were very big. 

To big to change the data to insensitive by hand.

In the end, I did not do any unit testing for them.

And yes, many bugs I had to fix more than twice. With every major, or even minor change to the code. But this post is not about the benefits of unit testing.

It's about the idea I had after that.

What if I had a script that could remove the sensitive data with little preparation from my side, and not touching the file structure?

And so [Deprivatizer](https://github.com/klimcio/deprivatizer) was created.

My goal is to make it possible for this script to deprivatize XML files. For starters.