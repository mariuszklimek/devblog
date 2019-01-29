---
layout: post
id: 187
title: Made a Jekyll site and realised it doesn't render the CSS properly
date: 2019-01-26T00:03:23+00:00
author: Mariusz Klimek
permalink: /jekyll/css-does-not-render-on-github-pages/
categories: jekyll
---
# TL;DR

In this series of posts, I will document how I created a blog on GitHub Pages.

In this post, I'll describe my first problem in creating a blog with Jekyll and how I solved it.

## **Why leave WordPress?**

If you're fed up with WordPress sites (like me), want free hosting for a blog, quite nice domain name for free and have a DIY blog for yourself, you might want to try out GitHub Pages (gives you free hosting) and Jekyll (a static content site generator). As long as it is all in **static pages** you can make a quite nice site. It struck me like a lightning: do we really need a database for blogging?

So...

## Making a blog with Jekyll

I created a repository on Github.

At first, I tried to make the blog with Theme Chooser. I chose the minimal theme and saved the default file. Sure, it looks good. Then I saw the file structure:

* _config.yml
* index.md

That's it.

Someday, I will probably create a Jekyll page from scratch, but to be honest, I'd like to jump start into blogging. So I started from the beginning. Using [quickstart guide](https://jekyllrb.com/docs/) I created a basic template.

`jekyll new myblog`

Just like that. But after I committed it I saw that none of the CSS is being rendered. Few days of googling (srsly) I found the [solution](https://stackoverflow.com/questions/33634520/jekyll-on-github-pages-not-rendering-css). I modified the **baseurl** property in the config.yml file:

```txt
    baseurl: "/devblog" # the subpath of your site, e.g. /blog
```

Where devblog was my repo name.

Now, it's time to do some migrations :-)