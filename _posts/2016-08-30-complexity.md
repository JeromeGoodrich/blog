---
layout: post
title: The Crisis of Complexity
date: 2016-08-30
---

The biggest problem in maintenance of software systems is complexity. Software systems are
difficult to understand. There are 3 main contributors to the complexity of software.

1. State
2. Control
3. Code Volume


And there are two general approaches to understanding software.

1. Testing
2. Informal Reasoning

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
