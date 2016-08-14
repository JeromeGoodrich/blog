---
layout: post
title: Threads and Thread Pools
date: 2016-03-17
---

Tuesday was reminiscent of Monday with slow but steady progress being
made in a stupor. Wednesday was all about debugging and in the process
of doing so getting a better understanding of threads.

In Java, threads are independent paths of execution within a program. A
multithreaded program like my server, allows for multiple concurrent
tasks, like when multiple clients want to access the server. There are
two ways to create threads in Java

1.  implementing the Runnable interface
2.  extending the Thread class

A class that implements the Runnable interface has the method run(),
which contains the logic of the thread. a thread (lowercase "t") is an
object of the Thread (uppercase "T") class. threads are creating by
passing in a runnable object to the Thread constructor, and then
runnable once the start() method is called.

```java
RunnableClass runnableObject = new RunnableClass
Thread thread = new Thread(runnableObject);
thread.start();
```


When extending the Thread class, we override the run() method in the
class extending thread. This subclass can also call a Thread constructor
explicitly in its own constructor to initialize a thread by calling
super().

```java
public class myThread extends thread {

  myThread(String threadName) {
    super(threadName);
    start();
  }
}
```


When working with many threads it's possible for different threads to
access and attempt to change the same shared data simultaneously, this
is called a race condition. the JVM has it's own thread scheduling
algorithm, which means we don't know when or in which order the threads
will attempt to access the data. The data is therefore dependent on the
thread scheduling algorithm e.g. the threads are "racing" to
access/change the data.

Race conditions can be difficult to detect, so multithreaded unit tests
are essential. When a race condition is found it can be handled in
several ways.

-   creating stateless classes
-   using immutable objects
-   side-effect free functions

These aren't always possible though so it's suggested in Java to do the
following:

> -   Use atomic classes in java.util.concurrent.atomic package
> -   If mutation is required, dedicate one thread for doing
>     mutation(changing values of variable). This is also known as
>     actor-based concurrency
> -   Caution – Non synchronized collections are ALWAYS prone to
>     race conditions. Use concurrent versions of those collections.
>     e.g. prefer ConcurrentHashMap over HashMap
> -   Use appropriate synchronization in below sequence of preference
>     -   Reentrant read-write lock
>     -   Synchronized block
>     -   Synchronized method
> -   Sincere code reviews
> -   Testing in multi-threaded environment
> -   Follow all above religiously

**Threadpools**

If my server became insanely popular and I had tens of thousands of
clients all using the server simultaneously. I would be in trouble and
my server would likely crash. Without a limit on the number of threads
my server can reliably create and execute at a given time, It's possible
to crash the server with an unbounded number of threads running
concurrently. Enter the thread pool. A thread pool is almost exactly
what it sounds like -a pool of threads that a program can draw from.

Java creates thread pools with the Executors  class that provides a
multitude of different types of thread pools to create. I used a cached
thread pool in my server implementation, which creates new threads for
the pool as they are needed and reuses old threads as they become
available. Thread pools work by creating a set number of threads
initially, and then selecting a thread to run when the
execute(runnableObject) function is called.

Concurrency is a pretty expansive topic, but hopefully this provides a
better basic understanding of threads and thread pools

 
