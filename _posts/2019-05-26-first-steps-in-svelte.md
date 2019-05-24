---
layout: post
title:  "First steps in Svelte"
permalink: /svelte/firststeps/
categories: svelte tutorial
---

## Why Svelte all of a sudden? What about Electron?

Yo, hold up a minute! Wasn't I suppose to continue an Electron tutorial?

Well, in a way, I am. Since making desktop app with Electron is all made with webpage tools I will need something anyway. Although I could do it in jQuery, the JavaScript library I know best ;-) But I want to try something new. And something different than Angular / React / Vue. I will probably cover also some new Ecma Script 6 features, that I will learn along the way.

In a way, Svelte resonates with me more than the other mentioned frameworks. The idea to create a JavaScript compiler that takes your code and converts it to optimised JavaScript in the build time, rather than at runtime seems like the way to do it right. **Your customer should not pay the performance cost of the framework's abstractions!** Not to mention the penalty when your app loads for the first time. Let's say you create a Single Page Application and your client uses a rather old phone. It works bad, what excuse do you use? It works on my machine? Buy yourself a better phone? Dear developer, shouldn't you be more inclusive?

When trying to learn Svelte I had to realise my way of thinking about JavaScript apps is wrong. I'm used to the jQuery way: where you can write from scratch, no generators, just by coding. Not today, I suppose. You could, but the ammount of stuff you have to remember can be overwhelming.

I will create an entire app in Svelte. Maybe some day I'll cover how to make simple components and reuse them in a different app, not this time.

## Coding time! Getting started with Svelte

Well, let's start. In node terminal console write:

`npx degit sveltejs/template my-first-svelte-app`

At first, when I saw this line, I felt overwhelmed. I did not not any of the commands, I was terrified. I learned a bit about node, but this line was just black magic. After some complaining, weeks of discouragement - I decided to learn what it meant ;-)

And so...

* **npx** is a rather fresh addition to npm that allows you to execute npm package binaries without installing them. It's perfect for all CLI tools you need to generate code.

* **degit** will make a copy of a git repository. It will copy the contents of master branch, but much faster because it will not copy the git history.

* **sveltejs/template** is the git repo we're copying. It's a very simple Svelte application with Rollup config in it.

* The template will be cloned to a directory we called **my-first-svelte-app**.

Next thing to do:

`npm i`

To install all npm dependencies. Once finished you will notice that there are only devDependencies. Svelte is a compiler, right? You don't have to deploy it with your app.

We have the very basic app in Svelte already here. To launch it execute:

`npm run dev`

And go to [http://localhost:5000/](http://localhost:5000/).

## What's next?

Writing some actual Svelte applications :-) And using that knowledge when writing Electron desktop applications. This and some learning of EcmaScript :-)