---
layout: post
title: Memory Part 4 - Virtual Memory
date: 2016-08-17
---
The main memory of a computer is organized as an array of byte-size cells. Each byte has its own
unique physical address. The first byte has an address of 0, the second an address of 1, and so on.
Let's say we have a load instruction that reads a 4 byte word starting at address 4. When a CPU
executes that load instruction, it generates a physical address and sends it over the memory bus to main memory. The main memory fetches fetches the 4 byte word starting at address 4 and returns it to
the CPU, which stores it in a register. This approach is known as _physical addressing_. With
_virtual addressing_ the CPU accesses main memory by generating a _virtual address (VA)_, which is
then translated into a physical address before being sent to main memory. The process of address
translation is mostly handled by a dedicated piece of hardware on the CPU chip called the
_memory management unit (MMU)_.


Virtual memory, like main memory is a organized as an array of byte size cells stored on disk
_(remember that the disk is lower in the memory hierarchy than main memory)_. Each byte has a unique
virtual address that serves as an index into the array. information within the array is cached in
main memory. As with any other cache, the lower level (the disk) partitions the information it holds
into blocks, which server as transfer units between the disk and main memory. In a VM system, these blocks are called _virtual pages (VP)_, and similarly, physical memory is partitioned into
_physical pages_ or _page frames_.

At any point in time a VP could in any of the following states:

  - **unallocated:** The page has yet to be created by the VM system. Unallocated blocks are devoid
                 data, and thus don't occupy any space on the disk.

  - **cached:** allocated pages that are currently cached in phyiscal memory

  - **uncached:** allocated pages not cached in physical memory


As with any cache, the VM system must have a way of knowing if a VP is cached somewhere, and if it
is, it must know which phyiscal page it is cached in. These capabilities are provided through the
use of a _page table_. A page table maps virtual addresses to physical ones and is organized as an
array of _page table entries (PTE)_. Each PTE has a valid bit and an address field that indicates
the start of the corresponding physical page in main memory where the virtual page is cached.
if the valid bit is not set then a null address indicates that the virtual page has yet to be
allocated.


![VM Caching](http://www.programering.com/images/remote/ZnJvbT1jaGluYXVuaXgmdXJsPWNtYnc1U1ppSm5Nell6TTFJRE56a3pNeDhWTnlFak55Y2pOeThpTnk4aU13UVRNd0l6TDA1V1p0aDJZaFJIZGg5Q2RsNW1MNGxtYjFGbWJwaDJZdWMyYnNKMkx2b0RjMFJIYQ.jpg)


This is an example of a page hit, where the reference to a word in `VP 2` is a hit, and gives a
basic idea of how VM caching works.


This will be the final post for now in the memory series. VM has a lot more to it, and I urge the
reader to look into it themself. the primary source for the memory series was
_Computer Systems a Programmer's Perspective_.
