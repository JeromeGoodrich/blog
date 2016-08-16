---
layout: post
title: Memory Part 2 - Caching
date: 2016-08-15
---

Yesterday, I talked a bit about the memory heirarchy and the the idea behind caches. Today, I want to delve much deeper into
what caches are and how they work.


A cache is simply a smaller faster storage device that acts as a staging area for the objects stored in larger slower device.
Since we are dealing with a memory hierarchy, we can think of this in terms of levels. To go along with the diagram below, we'll
refer to the level the cache is at, as _k_ and the level of the larger slower device as _k + 1_.


Using a cache is known as caching. When caching, the  data in the _k + 1_ level get broken up into contiguous blocks.
Each block has its own address or name that sets it apart from the other blocks. Blocks are typically fixed in size, though
in certain cases like HTML files stored on web servers the block sizes can be variable. The device in th _k_ level acting as
the cache also has its data split into blocks the same size as the blocks in the larger slower device. At any point in time
the cache will contain any subset of the blocks in the larger slower device. Data between the cache and the device it is
caching is exchanged via block-sized _transfer-units_. The block size is fixed between levels _k_ and _k + 1_,
but in the instance where the device in _k + 1_ is acting as a cache for an even larger slower device in _k + 2_ the block size might
change. This makes sense because typically devices lower in the hierarchy have longer access times and using larger block sizes
helps reduce the impact the length of those access times.

![Caching](http://image.slidesharecdn.com/memory-hierarchy3450/95/memory-hierarchy-40-728.jpg?cb=1271312831)

In caching parlance, a hit is when a program looking for a data object stored in _k + 1_, first looks in its cache at _k_
and finds it. In this situation the program reads the data directly from the cache, which by nature of the memory heirarchy is
much faster than reading it from _k + 1_. Programs with good _locality_ will have a better chance at cache hits.


On the other hand, if the same program looks in the cache an doesn't find the data it's looking for, it's a cache miss. In this
case, the cache in  _k_ will fetch the requested object from _k + 1_, which might overwrite one of the blocks it's current blocks.
This is called _replacing_ or _evicting_ the block, and as such, the block being replaced is often referred to as a _victim block_.
The block that is replaced is determined by the cache's replacement policy. For instance, a cache might have a
_random replacement policy_ wherein it's blocks are replaced randomly, or one where the least recently used block is replaced.


Caches work and are so widely implemented because they do a great job at exploiting locality at both the temporal and spatial
level. _Temporal locality_ is just a fancy term for the fact that the same data objects are likely to be reused multiple times.
Caching exploits this because once a data object is copied to the cache, we can expect a subsequent number of hits on that object
Since reading from the cache is often much faster than reading from storage at the level beneath it, the subsequent hits are usually
going to make up for the original miss. _Spatial locality_ is a similar concept. Because blocks typically contain multiple data
objects we can expect that the cost of copying that block will be reduced by subsequent calls to data objects within the same block.
