---
id: 68
title: Bootstrapping classes in PHP
date: 2016-10-20T21:31:41+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=68
permalink: /php/bootstrapping-classes/
dsq_thread_id:
  - "5239716365"
categories:
  - Creating a REST API
tags:
  - php
  - PHP bootstrapping
  - unit-testing
---
# **What took me so long?**

I stopped writing for a while, mostly because I felt uncomfortable with the fact, that everything I describe would be someone else's blog post. Every time I want to write about my progress I feel like I’m overusing the fact that I stand on the shoulder of giants. It’s true, ofcourse, but isn’t that enough – should I write my own blog posts just to quote and link other people’s work? I was not sure. [So I asked](https://twitter.com/jsonmez/status/787301602885443586).

## The deal is... never think about includes and requires

There’s this reoccuring moment… every time I create a site in PHP. Importing all those *.php files. Y’know, what I’m talking about – **this one enormous file with all those includes and requires**.

A friend of mine suggested a way how to do all of this better. He pointed me to [this site](http://jes.st/2011/phpunit-bootstrap-and-autoloading-classes/). And I end up using all of what I could find. Who knows, perhaps it will lead me to a nice Dependency Injection Container.

I like the way this solution actually forces some good practices: one class per one file, class name same as file name. Someone said in the comments that it would be good to make the file name look like classname.class.php, but I didn’t go that road, since I decided to minimize the not-classes PHP files.

Now that I have a class that will load me every directory, I create a boostrap file, just like instructed. Only difference, I made two of them – one for tests, the other for “production”.

Why two bootstrap files then? The bootstrap_test.php file does not load Controllers from my API.

```php
<?php
include_once('Infrastructure/Autoloader.php');

include_once('constants.php');
include_once('database_connection.php');


AutoLoader::registerDirectory('Infrastructure');
...
?>
```

First I load the Autoloader, then I load the constants and then the PDO object. And then I load **every** directory.

The truth is, not everything I do in PHP right now is actually declared in classes. I have static fields, defined constants and a PDO object that I keep as a global variable. Something tells me, that I should make these object oriented too.

That’s all that is to it.

All credits for the knowledge I used here goes to [Jess Telford](http://jes.st/). And special thanks to [John Sonmez](https://twitter.com/jsonmez).