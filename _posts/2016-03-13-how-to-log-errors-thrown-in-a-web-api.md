---
id: 54
title: How to log errors thrown in a Web API
date: 2016-03-13T21:54:02+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=54
permalink: /creating-a-rest-api/how-to-log-errors-thrown-in-a-web-api/
dsq_thread_id:
  - "4660359782"
categories:
  - Creating a REST API
tags:
  - Exception
  - php MySQL
---
Debugging a web API would be hard, I suppose. People who will use your site will generate errors, and we cannot be sure we will be able to reproduce them easilly. You might never learn what actually happened. Unless… you were prepared for that. Unless you thought of some kind of error logging.

# **What will we log?**

Exceptions, mostly. That’s what I’ll cover in this post. You could ofcourse log much more. Whatever you like, make that site visitations, search phrases entered by the user or any user launched event. But that’s for you to decide. Right now, we’ll cover the exceptions thrown by the code.

# **Where’s the catch?**

Not that kind of catch  ;-) The try-catch block.

Since I have a bit created in [my attempt to create a REST API](http://blog.mariuszklimek.eu/category/creating-a-rest-api/) (although it’s much more of a Web API right now), I will base the exception handling on that code. And since [I modified the .htaccess file to reroute almost everything to a single file](http://blog.mariuszklimek.eu/creating-a-rest-api/custom-routing-in-php/), I decided to put only one catch block to catch all exceptions. At the very heart of the API. To put it simple, it would look something like that:

```php
try
{
  // do some stuff
}
catch(WebApiException $wae)
{
  $json_result = prepareResult($wae->getMessage());
}
catch(Exception $e)
{
  $errorLogger = new ErrorLogRepository($pdo);
  $logResult = $errorLogger->Insert($e);
  $json_result = prepareResult($e->getMessage());
}
```

Easy, huh? But before I tell you what is inside the ErrorLogRepository...

# **What's WebApiException?**

It’s a custom Exception I created for catching several exceptions I throw myself in the API’s logic, like validation errors, and every error I do not want to log.

If the user enters an invalid date, and I show him an error message, should I log this in the error log table also? I don’t think so, what I want to log, are Exceptions that I did not oversee. Validation errors will be fairly common, logging every one of them will fill my table very fast, and won’t be very useful.

What’s the exception code? It’s very simple :-)

```php
class WebApiException extends Exception { }
```

# **Where do we store the error logs?**

It’s a simple table, that will contain all the data from the Exception.

```sql
CREATE TABLE Error_Log (
  id int(11) NOT NULL AUTO_INCREMENT,
  added_on timestamp default current_timestamp,
  message varchar(100) NOT NULL,
  file varchar(100) NOT NULL,
  line int(11) NOT NULL,
  stack varchar(5000) NOT NULL,
  PRIMARY KEY (id)
);
```

Nothing very complex here. The ErrorLogger class makes a simple SQL Insert to this table.

# Summary

As I learned developing this WebAPI – it can get very hard when debugging. If someone files a bug, I will have to reproduce it somehow. WebApiExceptions will be easy to trace since I throw them only in specific circumstances and with a unique error message. But sometimes unexpected errors occur, and this where this new table is comming handy.

There is one more step I need to do in order to make my API more flawless and predictable – unit tests.

Once I extract functionalities and logic from the controllers, my application will throw less and less exceptions. And that should make it even more maintainable. But that is something I’ll cover in an other post.