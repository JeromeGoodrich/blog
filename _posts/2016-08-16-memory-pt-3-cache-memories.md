---
layout: post
title: Memory Part 3 - Cache Memories
date: 2016-08-16
---

The previous posts in this series focused on the concept of caches and caching in general terms. This post will focus on
_cache memories_ which is a specific type of cache.

We briefly touched on why cache memories exist in [Part 1](). To referesh your memory (no pun intended) cache memories are
an attempt to bridge the _processor memory gap_. Essentially, CPUs/Processors have gotten faster and cheaper at a rate that
completely dwarfs that of DRAM (Direct Random Access Memory) and disk technology. and this divergence in performance and price
has led to the some innovative ways of bridging the gap. Most notably, the use of SRAM (Static Random Access Memory) based caches
which leverage the concept of locality explored in the previous post.


The modern computer system is typically equipped with three SRAM cache memories: L1, L2, and L3, which are located between the
register and main memory in the memory hierarchy. The L1 cache is the smallest and fastest of the three and can be accessed
nearly as fast as the register (~ 4 clock cycles). The L2's access time is around 10 clock cycles and L3's around 50.


Let's imagine we are taking a look at a simple system with only an L1 cache. The cache is organized as an array of _x_ number of
sets, with each set having _y_ number of lines. Each line constains a valid bit, a data block of size _z_, and tag bits. The valid bit
indicates whether or not the line contains any meaningful information. The tag bits uniquely identify the block that is stored
in the cache line. Imagine we want to read a _word_ from main memory. The CPU will send the memory address to the cache and see
if the cache already contains the data, but how does that actually work? How does the cache know whether it contains a copy of
the requested information?


Memory addresses are just sequences of bits, the parameters of number of sets _x_ and block size _z_ partition those bits into
3 parts shown below. The _set index_ bits relay which set the cache should look in. the _tag bits_ reveal what line in the
set (if any) contains the word. The line only contains the word, if and only if the valid bit is set and the tag bits in
the address match the tag bits in the line. If all of that checks out, then the _block offset bits_ give us the offset of
word in the data block.

![cache organization](http://people.engr.ncsu.edu/efg/521/s06/common/lectures/notes/lec4_files/image016.jpg)


There are many more specifics to implementation of cache memories, but for the purposes of this post and series I think I'll
stop here.
