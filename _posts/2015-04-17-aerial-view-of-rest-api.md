---
id: 5
title: An aerial view of my REST API
date: 2015-04-17T23:44:25+00:00
author: Mariusz Klimek
layout: post
guid: http://blog.mariuszklimek.eu/?p=5
permalink: /creating-a-rest-api/02/aerial-view-of-rest-api/
dsq_thread_id:
  - "3731490860"
categories:
  - Creating a REST API
---
Back to the idea of creating routing in PHP.

# The routing grammar

I remember a routing in ASP.NET MVC. There was the Controller Name, Action Name and the Parameters. The action name could be missing when we want to use a default action.

In my case, if I wanted to get a single article (which I consider a default action) I could make the grammar to accept something like that:

//_api/Article/5

Where "5" is the article id.

In the beginning I wanted to make the routing exactly in the same way. Then I did some thinking only to realise I would only produce extra code for that. It's a REST API, it will be called inside some code (at least that's how I want to use it), so obviously it doesn't have to be very flexible.

If that was the routing for a website then giving the user the ability to go to me.com/Article/5 would be very convenient. And this is probably how I will create routing for the site. But this time, I decided to go with the full REST URI.

## Aerial view of my REST API (GET)

It turns out that the basic code of the REST API I developed looks like this:

```php
try
{
    $cutFromHere = strpos($_SERVER['REQUEST_URI'], $_REQUEST['request']);
    $url = substr($_SERVER['REQUEST_URI'], $cutFromHere);

    $routeInterpreter = new RouteInterpreter();
    $routeInfo = $routeInterpreter->parse($url);

    $controllerName = $routeInfo->Controller.'Controller';
    $controller = new $controllerName($pdo);

    $result = $controller->{$routeInfo->Action}($routeInfo->Parameters);

    $json_result = json_encode(array("IsValid" => 1, "Result" => $result));
}
catch(Exception $e)
{
    $json_result = json_encode(array("IsValid" => 0, "Result" => $e->getMessage()));
}

echo $json_result;
```

That's surprisingly simple (for GET anyway).

  1. [Line 3, 4] We take the URI and we cut the part which has the site name. We ain't gonna need that.
  2. [Line 6, 7] We call a RouteInterpreter class and we parse the URI inside it. The result is an object which contains the controller name, action name and an array of parameters (or empty array if no parameters are provided).
  3. [Line 12] Then we use some cool PHP magic to actually call a class' method based on a string I keep in the property Action. Very neat solution, if I can say so myself :-)

Some decisions had to be made.

* I didn't have an idea how to solve the parameters case, so I decided to pass a whole dictionary inside. Once inside the controller's method I would have to check for the parameters I'm interested in. If I find a better solution to this one I will share it down ofcourse :-)
* [Line 14, 18] Why is there an array with two values as the return? I learned that one earlier in my developer career. When I receive the data from the REST API I can check if everything went well. IsValid property gives me that information, it's a simple boolean answer. The Result will contain the array of information, or the thrown exception.

## **What next?**

* Well, I created the RoutingInterpreter, so I believe I will describe it next.
* Then there will be some humdrum work when creating GET requests for every entity used in my REST API. I believe I will describe the first one.
* Then... the biggest challenge so far - POST requests :-)