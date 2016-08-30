---
layout: post
title: Complexity of Control
dates: 2016-08-29
---


Consider the following pseudo code:

```
a = 7 + b
c = 5 * d
e = 1 - f
```
What's going on? Give it a sec what do you notice?


Nothing, right! We've simply established some relationships between variables, and that's it.
Well, not quite we've also specified an arbitrary ordering to these expressions, by virtue of
the semantics of the language. With this trivial example, it's easy to see that the ordering
is of no consequence to the meaning of the program, but with less a less trivial example, how we
choose to order things can greatly increase the complexity of the code.

> This is because the person reading
the code above must effectively duplicate the work of the hypothetical compiler
— they must (by virtue of the definition of the language semantics)
start with the assumption that the ordering specified is significant, and then
by further inspection determine that it is not (in cases less trivial than the
one above determining this can become very difficult). The problem here
is that mistakes in this determination can lead to the introduction of very
subtle and hard-to-find bugs. - Mosely & Marks (_Out of The Tarpit_)

It's worth noting that this isn't the case with all languages, just imperative languages with
guarantees about the order of execution.

Another interesting/scary thing related to control-flow is concurrency.

> Running a test in the presence of concurrency with a known initial state and set of inputs tells
you nothing at all about what will happen the next time you run that very
same test with the very same inputs and the very same starting state. . . and
things can’t really get any worse than that.

So true, the complexity introduced by control flow and how things are ordered is no joke.
It makes our programs very difficult to reason about and test. This example specifically points
to the fact that tests are far from infallible. At best, they can increase your confidence in the
behavior of a sytem i.e. that id does what you'd expect it to, but there is no certainty, and we
have to constantly operate under the only assumption we can make - That we are not standing on solid ground.
