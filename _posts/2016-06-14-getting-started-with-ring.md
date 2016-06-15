---
layout: post
title:  "Getting Started With Ring"
date:   2016-06-14
---

After about 5 months of working of pairing on Rails clone of ebay, I am starting a new project. I am going to build a [request bin][1].
A request bin provides a user with a url that they can make requests to. The user can then see all the requests made to the url in
an easy to manage UI. I'll be building the request bin in Clojure. As such, the first part of this project is really about familiarizing
my self with the "Clojure Stack" the foundation of which is Ring.


Ring is defined in terms of handlers, middleware, adapters, requests maps, and response maps. Ring represents both requests and responses
as clojure maps with certain required keys and certain optional ones, because request and response are implemented as regular clojure maps
they can be processed and augmented like any other map.


Handlers are simply functions that take a request map and return a response map. That's it. The functionality of handlers can be modified and
extended through the use of middleware, which act as wrappers to any number of handlers. Middleware in Ring, is simply a higher order function
that takes any number of handlers as an arguments and returns another handler with the desired augmented behavior.


When an http request is received, the Ring adapter takes the request, deconstructs it into a request map and passes it to the ring application to be processed.
The application returns a response map which the adapter then reconstructs into a response before sending it off to the server.

[1]: http://www.requestb.in
