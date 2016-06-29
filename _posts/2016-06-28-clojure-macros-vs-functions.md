---
layout: post
title:  "Clojure: Macros vs. Functions"
date:   2016-06-28
---

So I've got a bunch of functions that have this ugly try-catch logic in them here's an example of one
of those functions:

```clojure

(defn get-something [id]
  (try
    (db/get-something id)
    (catch Exception e
      nil)))

```

Basically, I'm wrapping some database operation and introducing some error handling with a default-value. The try-catch
is really ugly though and repetitive though so I'd like to somehow extract it and reduce the duplication. Simple enough,
I'll just write a private function that takes a function and a default-value as arguments. Problem solved...

```clojure

(defn- with-error-handling [default-value fn]
  (try
    fn
    (catch Exception e
      default-value)))

```

I'll run my specs to verify and... Hey! what gives I have a bunch of failures. That clearly didn't work, but why?
Well the issue is in how I'm passing in the function. if we look at the example below with of the `get-something` function using
my new `with-error-handling` function we see that I'm calling `db/get-something` with the id params this evaluates that function
and passes the result to `with-error-handling`. To little too late, the exception has already been raised by calling `db/get-something`
and so `with-error-handling` does not do the job, for which it was created.

```clojure

(defn get-something [id]
  (with-error-handling nil
                       (db/get-something id)))

;; This is the same as

(defn get-something [id]
  (with-error-handling nil
                       some-exception))

```

In Clojure, functions transform values into other values. In our case, we actually want to transform our code into something else.

```clojure

;; We want this
(db/get-something id)

;; To turn into this
(try
  (db/get-something id)
  (catch Exception e
    default-value))

```

Enter macros, which do just that. transform code into some other kind of code. This makes macros extremely powerful, In fact, if you felt so inclined you could
write an entirely new programming language using Clojure macros. But first, back to the problem at hand.


I want to create a `with-error-handling` macro. how do I go about it? To start, It probably helps to understand how macros transform code into different code
and for that we probably need to dive deeper into the bowels of Clojure. I'm saving that exploration for the following blog post. Until next time...
