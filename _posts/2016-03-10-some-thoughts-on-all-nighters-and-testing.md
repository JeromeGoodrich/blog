---
layout: post
title: Some Thoughts on All-Nighters and Testing
date: 2016-03-10
---

I pulled an all nighter last night with the foolish intention of making
more progress on my requirements before my IPM today. I stayed late at
the office, wrestling with my understanding of  how to create new
interfaces and mock out objects to make the "server loop" in the my Java
http server more easily testable. I had already devoted ample amount of
time to working through the issue and actually left the office at a
decent stopping point. Unfortunately, and predictably, I decided to keep
working. In fact, I actually went to the supermarket on my way home to
pick up a RedBull, having already determined that tonight was going to
be an all-nighter.  Insane... I might have done 30 minutes of actual
work the entire night. The rest was spent trying not to fall asleep
because the "RedBull was going to kick in any second now" and watching
youtube. Now I've robbed myself of an afternoon and (potentially)
morning of productivity and have very little to show for it -though
there were some pretty funny videos I saw.

I'm writing this to remind myself, and anyone else that has a proclivity
towards voluntary and misguided sleep deprivation not to give in to it.
 It's much healthier, and much more professional to walk away and tackle
things anew when feeling fresher.

. . .

Through my apprenticeship, I've been continually surprised and vexed by
how difficult it is to write good unit tests. Personally, I find it
helpful to think of tests as assumptions I am making about the design of
a particular piece of software. It's me saying "based on
my *understanding* of object X  and it's properties I'd expect it to
exhibit or not exhibit this behavior . Now, what can I do to verify that
it is exhibiting the behavior I'd expect".

Therefore the prerequisites to writing a unit test that correctly
asserts and verifies an assumption are rather staggering. It takes a
full understanding of an objects properties and behaviors as well as an
understanding how how those properties and behaviors can be verified.
Take writing unit tests for my 'server loop" as an example.

In order to write a satisfactory unit test, I must understand, what a
server is, what a server does, what a service is, and what it does, what
a client is, what a client does, how servers and clients interact
through sockets, what Sockets are and how they are implemented in Java.
It's already a lot to wrap your head around, and then on top of that I
must also understand that in order to verify the behavior I'm expecting
I need to simulate the behavior of the sockets and service in other
objects called mocks, which requires building an interface and other
abstractions. It's a lot.

The good news is that when I do finally write a satisfactory unit test,
I can be pretty sure I understand everything that's going on under the
hood. However, this usually comes through designing first as writing
tests to verify the design. Eventually, I'll need to be writing tests
first. There's still a lot to learn, but I'm starting to understand not
just intellectually but empirically the value of testing.
