---
id: 11
title: Custom routing in PHP
date: 2014-09-07T00:01:00+00:00
author: Mariusz Klimek
layout: post
permalink: /creating-a-rest-api/01/custom-routing-in-php/
dsq_thread_id:
  - "3690819743"
categories:
  - Creating a REST API
tags:
  - .htaccess
  - php
  - routing
  - wampserver
---
# Introduction

I have this site written in PHP. It's a sort of personal proving ground. I designed it, made every possible design and programming mistake, and learned a lot in the process. It motivated me to learn PHP and MySQL. Every time I learn new technologies or web practices I think about the profits my site would gain from them. As you can imagine - it's not very readable. Proving grounds are bad for code readability and maintanability. Especially when you learn a language only for the needed things, and that's how I learn PHP to this day.

Ever since I learned about ASP.NET MVC, I was thinking about implementing the MVC pattern for this site. I liked everything about the whole concept, especially the routing. But I didn't want to rewrite the whole site to some existing framework. I was worried, that most of the code would be some sort of foreign body, out of my control and understanding. The site already has a WordPress blog, and modifying it's template to make it look as an integral part of site was kind of like drawing a graffiti on a monumental wall. It was a mess.

And then, quite recently, I learned about REST API's. Something I need to learn for my every day work. What's the best way to learn it? I know a site, that would profit from it :-) After reading some articles on the Internet on how to implement a RESTful API in PHP, I learned about .htaccess files. Now I know how to do a routing in PHP, and therefore... maybe implement my own small simple MVC pattern to the site?

## Concept

The key to a custom routing in PHP lies in adding a .htaccess file. That's it...

### Implementation

I added an "API" folder in the website folder structure. There I created a .htaccess file. It's content is based on what I found in a text by [Corey Maynard](http://coreymaynard.com/blog/creating-a-restful-api-with-php/"):

```txt
<IfModule mod_rewrite.c>
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?request=$1 [QSA,NC,L]
</IfModule>
```

Long story short, thanks to this configuration every request that starts with www.yoursite.com/api/ and points to a **non-existing** file or directory will be redirected to the index.php file. The $1 variable will be the request from to URI. It will be useful when interpreting and easy to get via $_REQUEST['request'].

The index.php file is on the same level as the .htaccess file.

## **Possible problems?**

The truth is, you might not have the specified module enabled.

For WampServer I needed to enable the module to make it work locally. One way is to navigate through the tray icon to "Apache/Apache modules", check rewrite_module and then restart all services. The other would be to edit the httpd.conf and uncomment the line with:

```txt
#LoadModule rewrite_module modules/mod_rewrite.so
```

I suppose one of things I should do for the site is to do some sort of self testing web pages, so I know that the people that host the site didn't change anything. It may sound paranoid and absurd, but then again, I needed to suddenly add "set names latin2;" for correct viewing of database imported data...

If you want to check if it's enabled on the server you can use this code:

```php
echo in_array('mod_rewrite', apache_get_modules());
```

## **What next?**

At this point I gained some very important knowledge that allows me to...

* Create an MVC framework for my websites. It will be better than using existing solutions, mostly because it will give me extensive knowledge about implementing the MVC pattern. With all the code being my job, I will have no expendable components.
* Proceed with creating a REST API for my websites. Now I will be able to actually separate my database logic from the website interface!

## Useful links

If you'd like to know which steps I took to find the solution, here are the sites from where I gained my knowledge:

* [Corey Maynard](http://coreymaynard.com/blog/creating-a-restful-api-with-php/")