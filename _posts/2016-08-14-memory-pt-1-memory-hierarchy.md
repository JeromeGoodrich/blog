---
layout: post
title: Memory Part 1 - The Memory Hierarchy
date: 2016-08-14
---

Computer's spend a lot of time moving information from one place to another. These operations are necessary,
but the longer they take the worse they are for a program and, therefore programmer. Since speed is such an important
component of any program, a lot of thought has gone into the design of how the memory of a machine is managed.

In the context of computer memory a general rule of thumb is that there is an inverse relationship between size and speed.
That is the larger a memory store, the slower it is and vice versa. It might also be worth noting that this realtionship is
seldom a linear one. A disk drive on a typical system might be 1,000 times larger than the main memory, but it might be 10,000,000
times slower reading a word from the disk than main memory. A typical register file (the smallest and fastest memory store)
might only store several hundred bytes of information, as opposed to a main memory that can story billions of bytes, but reading from
the register file is 100 times faster.

This relationship is known as the the _processor-memory-gap_. One innovation spawned from this issue is the use of _cache-memories_
that serve as temporary staging areas for information the processor is likely to need in the future. Modern systems contain a L1, L2,
and L3 cache. The L1 cache is the fastest and smallest and can access data almost as quicly as a register file, L2 is lsighly slower and L3
slower than that. While the SRAM (Static Random Access Memory) caches are specific hardware devices, the idea of a cache is a general one.

Caches are used to exploit the concept of _locality_ - the idea that programs tend to access data and code in localized regions - which allows
a system to benefit from both a large and very fast memory. How exactly, that happens will be discussed in later posts, but for now just know that
because of locality memory is organized in a sort of heirarchy where the memory at one level serves as a cache for the memory below it. Registers
are a cache for the L1 cache, which in turn is a cache for the L2 cache, which is a cache for the L3 cache, and so on...


![Memory Heirarchy](http://img.blog.csdn.net/20130711103254515?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQl9IX0w=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
