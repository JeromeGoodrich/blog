---
layout: post
title: My Elixir Introduction
date: 2016-08-22
---


After talking about it so much with my fellow apprentices, I finally got my
first taste of elixir and the Phoenix framework today while pairing at a client.My initial impressions of the language are extremely positive, because it is
dynamic it looks a lot like Ruby, but because it is functional it is was pretty easy to reason about and follow.


The pattern matching elixir provides is definitely one of the langauge's
hallmarks and through my work today I was able to see some of it's versatility
and power. Take the following example:


```elixir

Bypass.expect bypass, fn
  %Plug.Conn{request_path: "/home"} = conn ->
    assert something = something_else
  %Plug.Conn{request_path: "/users"} = conn ->
    assert this = that
end
```

The Bypass library is a light-weight, quick way to create a custom "plug" (Plug is the clojure Ring of the elixir world) in place of an actual HTTP server. It'sessentially a mock HTTP server without actually being a mock (apparently it's
somewhat frowned upon to use mocks in elixir). Anyway, all we are doing in the
code above is passing in a Bypass struct and an anonymous function that contains some assertions about the response we are expectiing from our fake Bypass
server

In elixir, Anonymous functions take the following form:

```elxir
fn
  parameter_list -> function_body
  parameter_list -> function_body
end
```
Thanks to pattern matching, the function can take multiple parameter lists to

provide alternate function bodies, which is exactly exactly what is happening inthe Bypass example above. There's still something strang though. The `=` sign is part of the pattern? Yeah, atually. We are basically saying, "if the parameter map we pass in has a `request_path` of "/home" then _assign_ it to the variable conn, and execute the assertions in the function body. Pretty sweet!

There are still quite a bit of nuance in pattern matching that I have yet to
understand, but what I see and understand so far has made me very excited about elixir. I look forward to playing around with it some more.
