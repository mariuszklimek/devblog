---
id: 155
title: How I stopped worrying and banned myself from social media on all my computers
date: 2018-08-03T21:46:41+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=155
permalink: /adventures-in-c-sharp/how-to-ban-yourself-from-social-media-on-all-my-computers/
categories:
  - Dev Diary
tags:
  - dns
  - hosts file
  - lifehack
---
# **Storytime**

Don't you just hate the moment, when you open a new tab in your browser and the first thing you type is "f" and suddenly you realize you are scrolling down on Facebook? Or when you realize you do that a few (or more?) times in an hour?

The first time I noticed something like that was when I was commuting by train. I usually read a book, but back then something was different - I used to wear a watch. And while reading a book I looked at the watch like every minute. I asked myself: why? It's not that it will help me arrive at work earlier than usual, I cannot bend time and space... I decided to lose the watch. I already have one on my smartphone, and I don't reach for it that often.

Flash forward. Today. I still don't wear a watch. Any new problems? Yes, I have the same problem with social media. On my home computer, on my work computer. I tried to solve that by installing blockers on browsers to block different kinds of social media. It works... for one browser. I still access social media on browsers I don't use for anything (Edge, anyone?). But sometimes I just open Edge. So I started to search for different solutions. And I found a simple one that works.

## **How to disable social media on your computer?**

Thanks to the Hosts file you can define which domain names (websites) point to which IP addresses. It's mega-powerful because it runs before DNS servers. I decided to use it to my advantage.

I entered the directory **C:\Windows\System32\drivers\etc **and edited the **hosts** file (no file extension).

Remember:

* Changes take place immediately, just save the file.
* Each record must be in a separate line.

I entered the file and added those lines at the end:

127.0.0.1 localhost
  
127.0.0.1 www.facebook.com
  
127.0.0.1 www.twitter.com
  
127.0.0.1 twitter.com
  
127.0.0.1 [https://twitter.com](https://twitter.com)

The first line was already there. I added the next four.

You can find richer examples on the Internet.

I decided to block myself from entering Facebook and Twitter newsfeeds - and this one works just fine.

What these lines do is basically redirecting the browser to my local computer from the location set at the end. You want to access facebook? No way! You will see localhost. I suppose I should edit my working web server on localhost so that it's default message will be "Get back to work!" ;-)

## **What else can the Hosts file be used for?**

Another gate to ultimate lifehacking has opened. That's not the only thing you can do with the Hosts file. Although, in my case, that's the only thing I use it for. Someday I will explore the new possibilities:

* You can create aliases to web pages of any kind.
* You can create your own local domains.
* Redirect URLs to different sites.

For now, this is it.

## **One month later...**

This change still works. I did it on my home computer and on my work computer. I access social media only on my mobile phone, which basically helps to focus a little more :-)