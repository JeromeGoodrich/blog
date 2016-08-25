---
layout: post
title: A Quick Primer on The Stack, Heap and Garbage Collection
date: 2016-08-24
---


I guess I couldn't really get away from Memory, and this is a topic that has
eluded me for quite a long time. Before this I only had a vague understanding of these concepts, enough to bs my way through a conversation, but I had never bothered to actually try and understand what they meant. Until now!


###The Stack

Everytime your application spawns a thread, a new stack is created. It's pretty much entirely managed by the CPU and adheres to LIFO (Last-in Firs-out). In other words things are pushed on to the stack and popped of the stack. Everytime a function declares a new variable, it is pushed on to the stack. When the variable falls out of scope, like when a function closes, the variable will be deallocated (popped off) from the stack automatically. The variables on the stack are always local since they are next in line to be popped off.

Stacks are fixed in size, and allocating more memory than the stack can hold will result in a stack overlfow. As such the stack imposes limits on certain types of variables like integers from ever growing beyond a certain size, or making more complex data types like arrays specify their size prior to runtime.

###The Heap

The heap is another memory store, but unlike the stack, it allows for dynamically sized data types and global variables. There's no notion of pushing or popping, it's just storage. When you allocate memory a memory location on the heap to store a variable, that variable is accessible at any time throughout the lifetime of the app. Unlike the Stack, the Heap does not have a fixed size, nor is it managed by the CPUi. As such, it's important to clear the Heap otherwise it will result in a loss of performance, or even memory failure. This failure to release discarded memory is called a _memory leak_. In programs like C and C++ programmers have to manually deallocate memory on the Heap, but luckily many other programs have automatic memory managers that can take care of this for us.


### The Garbage Collector

The garbage collector is a form of automatic memory management that attempts to free the memory space occupied by discarded objects that are no longer in use by the program aka. _garbage_. Basically, it is the garbage collector's role to find data objects in the program that cannot be accessed in the future, and to reclaim the resources used by those objects. Garbage collection can take a significant proportion of processing time in a program, so it's important to understand because it can really effect performance.


