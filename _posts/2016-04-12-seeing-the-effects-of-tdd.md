---
layout: post
title: Seeing the Effects of TDD
date: 2016-04-12
---

I'm gaining yet another new appreciation for TDD with this second go at
the HTTP Server. For the first time, I can actually *see* -I feel like
it's something I understand intuitively, but until now haven't actually
experienced (at least to this extent)- how tests can influence the
design of code. I started this attempt at the server with the entry
point of the program, the Main method, but it proved to difficult to
test, so I moved on to writing tests to configure the server. The next
logical step from there was to actually test the server loop and
following that the actual service, which I'm currently still testing. At
this point the program won't actually do anything if it's run (I
actually don't think it will run at this point as the main method isn't
written), but interestingly through writing out a total of 5 tests, I
have the majority of the scaffolding and design in place for the server.
It's like I've built a house of interfaces and abstractions for the
business logic to occupy based on some specs provided by the tests.
Pretty cool.

Other than that, I've gotten rid of the Vim emulator on top of
VisualStudio at the behest of my mentors. They say that it will confuse
me to learn Vim in something that isn't Vim. I don't have the experience
to argue and I did find it rather difficult when I was practicing the
kata so I'll stick with VS and ReSharper shortcuts for the time being.
