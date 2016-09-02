---
layout: post
title: The Crisis of Complexity
date: 2016-08-30
---


Complexity is greatest major difficulty when developing software systems. That sounds like
a platitude, but the underlying reasons from this difficulty are important and interesting to think about.
In [yesterday's post]() I touched briefly on complexity arising from control as I found it most surprising
When reading. However, in terms of impact, it pales in comparison to complexity arising from state, which
I'll talk a bit about today.


Before that, it's important to note that the issue of complexity in software, is an issue becuase
complexity makes it difficult to understand a system, and that we usually employ two general approaches
to understanding a system.


1. Testing
2. Informal Reasoning


Where testing is meant as a sort of outside-in approach to understanding the system. i.e.

State is the primary contributor of complexity in a software system because

> “From the complexity comes the difficulty of enumerating, much
less understanding, all the possible states of the program, and
from that comes the unreliability”

and

> “computers. . . have very large numbers of states. This makes
conceiving, describing, and testing them hard. Software systems
have orders-of-magnitude more states than computers do.”

We have a very difficult time understanding a system with state involved using testing becuase
a test of a system in one state doesn't tell us anyhting about the same system in a different
state. Similarly, testing with one set of inputs does not tell you anything about a system tested with
different inputs.

State also makes it very difficult to reason about the test. We are typically trying to balance, all the
different scenarios in our heads and as the number of states grows this becomes increasingly more diffcult.
For every new state we add we are actually doubling the total amount of possible states.
There is a problem of contamination, which happens when a stateless procedure makes use of (even indirectly) of
as stateful one, reasoning about it becomes substantially more diffcult as we can only understand it in the context of state.

I discussed the problem of control in my blog post yesterday.

Code volume is fairly self-explanatory.
