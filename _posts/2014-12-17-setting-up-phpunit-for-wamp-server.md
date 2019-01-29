---
id: 8
title: Setting up PhpUnit for WAMP Server
date: 2014-12-17T10:53:00+00:00
author: Mariusz Klimek
layout: post
permalink: /php/setting-up-phpunit-for-wamp-server/
dsq_thread_id:
  - "3690818440"
categories:
  - Creating a REST API
tags:
  - batch
  - Composer
  - PHP bootstrapping
  - PHPUnit
  - wampserver
---

# Introduction

I decided that I want unit testing in my php projects. It's kind of hard to start because I'm developing it under Windows, which - according to some of my fellow developers - is not the best environment for developing backends :-)

At first, it seemed to be not the easiest in configuration also. After going through some tutorials on the Internet I almost decided to leave PHP unit testing alone. It's actually quite simple. All you need is:

  1. Composer
  2. A PHP bootstrap file
  3. An xml test suite
  4. A bash file for executing tests

## Composer

Install [Composer](https://getcomposer.org/).

In your project main directory create a file called composer.json and fill it with this data:

```json
{
   "require-dev": {
      "phpunit/phpunit": "4.8.*",
      "phpunit/phpunit-mock-objects": "2.3.*"
   }
}
```

Executing Composer on this directory will create a vendor directory with the executables needed.

## A PHP bootstrap file

I don't want to take credit for explaining how to do bootstrapping properly, since all I learned about it you can find [here](http://jes.st/2011/phpunit-bootstrap-and-autoloading-classes). Just remember to put the bootstrap.php file in the main API directory.

## An xml test suite

In the Api folder I created an xml file which looks like that.

```xhtml
<phpunit bootstrap="bootstrap.php">
   <testsuites>
      <testsuite name="tests">
         <directory>Tests</directory>
      </testsuite>
   </testsuites>
</phpunit>
```

This is a test suite for every test declared in directory called Tests.

** A bash for executing tests

To simplify executing all the tests I decided to create a batch file. As every file created in this post it's in the Api main directory.

```batch
SET phprunner="../../vendor/bin/phpunit"
SET config="phpunit.xml"

cls
%phprunner% --configuration %config% --testsuite tests
```

So, what's happening here?

  1. First we set up a variable pointing to the phpunit executable (in my project I needed to go two directories higher.
  2. Then, I created another variable to point to the xml test suite.
  3. Then I clear the screen.
  4. And at last I'm launching the tests.

## **What next?**

First of all, I need to finish my little MVC project. And for that...

  1. I need to create a good routing algorithm.
  2. Create some basics for controllers and repositories.

This tutorial also suggested some topis for later:

* Learn about composer, since this seems to be to component that saved the day.
* Learn about Apache. My usual approach ("get something that's easy to configure or does not need manual configuration at all and start programming") failed me again. But some of the tutorials actually made me realise that I don't understand my environment. And this must change!
  
Oh yeah, one more thing. I really don't like the way I see the test results. Every time I need to launch it via cmd, which isn't too comfortable. So another goal for the near future is to get a good reporting feature (get one or make one). Since I will probably invest in jasmine specs for my websites, the best thing would be to have a html page view test results. This way I will be able to run all tests with one click :-)