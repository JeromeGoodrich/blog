---
layout: post
title: Compiled vs. Interpreted Languages
date: 2016-08-26
---


This is actually somewhat of a misnomer. Technically, all languages can be both compiled and interpreted.
What we're actually going to talk about is what these different techniques for executing code are and how
they differ.


In order for a program to be executed, it must first be translated into machine code that can be sent
directly to the processor. Compiling and interpreting are just different stratgies for transforming human readable
code like Ruby or Elixir into something the machine can read. When we say machine, this includes abstract machines like
BEAM or the JVM as well as CPU on your laptop.


A compiler will basically slurp up all the code you've written and translate into that machine readable code in one go.
For reasons that will soon become clearer, this makes compilation typically faster than interpretation since there's
just one step in between human and machine readable code. Compilation occurs for specific machine languages meaning that each
compiler is exclusive and optimized for compiling into only one language. This means that compiled code is not very portable or
platform-independent since it is dependent on a specific machine readable language. Furthermore, debugging can be a huge pain
when using compilation, because everytime you want to test a change in your program you must first recompile it.


Interpretation is using an interpreter to execute the code you've written line by line, which traditionally happens at run time. So
interpreted code has little to no start-up time but can be much slower in terms of execution time. To be honest, I'm still a bit fuzzy
on how interpretation actually works. It's clear that higher level code is translated into lower level code line by line, and then executed
by the lower level code, but how that execution occurs is when I get lost, since I think most lower level code typically needs to be compiled
anyway, so I guess I don't understand how the code is actually _executed_ in the case of interpretation.

In order to do thinspost justice I think I might need to delve into "just in time" compilation virtual machines and what the heck "runtime" actually
means. Stay tuned.
