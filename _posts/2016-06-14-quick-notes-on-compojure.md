---
layout: post
title:  "Quick Notes on Compojure"
date:   2016-06-14
---

Compojure is a routing library for Ring, that allows for the web app to be more composable, or modular.


Compojure has the `defroutes`, a macro that defines a var containing a single Ring handler (it takes a request map and returns a response map).
this handler dispatches to other Ring handlers via `routes`. Compojure routes consist of both a Ring handler
and the patterns used to match an incoming request to the handler. These can be broken down into 4 main componenets.

1. The HTTP method for which the Handler should be invoked.
2. The URI pattern that matches against the request's URI
3. A binding form for the request/params and matched portions of the URI - these become local vars in the Handler
4. The body of the handler

A route will either return a response or nil and the Ring handler contianed in the var defined by `defroutes`
will go through each route in order until it finds one that does not return nil.

```clojure

;It's worth noting that the post-handler and new-user-handler functions below is not a true Ring handler
;since they do not take a request map as its argument. The implications of this are inconsequential it seems.
;Also worth note is the response helper function from the ring.util.response namespace

(defroutes my-routes
  (HTTP-method    URI-pattern      binding-form     handler-body)
  (   GET         "/post/:id"       [url id]     (post-handler id))
  (   POST         "/users"           [url]     (new-user-handler url))
  (   GET            "/"              [id]      (response "hello world")))

```

Tomorrow, I'd like to dive in a bit into the ring-defaults library and middleware in general
