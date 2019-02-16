---
layout: post
title:  "First steps in Electron"
date:   2019-02-12 00:45:03 +0100
permalink: /electron/first-steps/
categories: electron tutorial
---

# **TL;DR**

1. Story time
2. Using Electron-CLI
3. Common problems with node tools in bash command line
4. All things made easy by Electron-CLI

# Story time

I decided to make a project in Electron.

I usually start by finding a simple tutorial. These days I use [Tutorials point](https://www.tutorialspoint.com/electron/index.htm). The tutorial isn't fresh and some parts can be done better, or only more comfortable, which I will point out.

# Using Electron-CLI

For starters, the tutorial prefers to show you [the hard way](https://www.tutorialspoint.com/electron/electron_hello_world.htm) when creating an Electron application. But I learned there is a better solution. You can install a great node tool - `electron-cli`.

I suggest to install it globally (as all tools):

`npm install -g electron-cli`

And then initialise the project:

`electron-cli init directory-name`

The CLI will create a directory, so perhaps it is better not to make a directory upfront if you're beginning app development.

And... that's it.

Although it was not just _that's it_ when I tried it at first :-)

# Common problems with node tools in bash command line

If you use Windows 10, as I do, you might encounter an error message like this:

`electron: command not found`

Searching for a solution I found [this](https://stackoverflow.com/a/35536453/1693915). It appears that you just need to add `%USERPROFILE%\AppData\Roaming\npm` to your environment variables and restart your terminal.

This time, that's it.

# All things made easy by Electron-CLI

Using it for the first time, you can observe some faciliations:

- The newly created project will already be a node.js application.
- A git repo will be already created.
- The main files (main.js, index.html, .gitignore and packages.json) are already committed with an initial commit.
- The main.js file will have comments describing what are all the JavaScript instructions doing.
- The main.js will contain an optional call to show Dev Tools to the user. Now you know how to call it.

Now, let's make an app :-)