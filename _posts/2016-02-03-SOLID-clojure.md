---
layout: post
title: SOLID clojure
date: 2016-02-03
---

I was digging around the internet trying to find a resource that might
help me better understand the SOLID principles, specifically in a
context related to Clojure and functional programming. Pretty cool, that
a it would lead me to [a talk by 8thlighter Colin
J](http://www.infoq.com/presentations/SOLID-Clojure). about this very
thing. Here are the principles (DILOS) as I remember them.

D - Dependency Inversion Principle

**Problem: **Changes in one part of code cascades to other parts of
code. Code is harder to change because there is more to change.
Typically caused by code that ought to change less, depending on code
that tends to change more.

**Solution: **Depend on abstractions not concretions. But what are
abstractions?

-   abstraction: defprotocol
    -   concretions: deftype defrecord
-   abstraction: defmulti
    -   concretion: defmethod
-   functions can also be abstractions that are "consumed" by
    higher-order functions
-   namespaces though not often

a cool thing about Clojure is that it many instances it forces you to
create the abstraction.

I - Interface Segregation Principles

**Problem: **Client shouldn't have to depend on abstractions they don't
use. Fat, uncohesive protocols and namespaces.

**Solution: **keep protocols and namespaces small and cohesive

L - Liskov Substitution Principle

**Problem: **A particular concretion is not substitutable for it's
abstraction. switching on types (conds and if statements depending on
details).

**Solution: **More of a human problem. It's about expectations, and how
we define them.

O - Open-Closed Principle

**Problem: **adding new features requires changes to existing code.

**Solution:** Use multimethods and other types of polymorphism in
Clojure

S - Single Responsibility Principle

**Problem: **Code is doing too many things at once. It's hard to reason
about. Hard to change because you need to think about it more. Reuse is
also impeded

**Solution:** Give a unit only one reason to change.

.  .  .

That's a really high-level overview, but it was really helpful to get
some more context around these abstract concepts in the language that
I'm currently working in. Specifically, because I'm at a point now where
I've got all the features and functionality working for tomorrow's IPM,
but I notice some places in the code that violate these principles and
now I have a better understanding of why, an idea of how to solve them,
and quite an itch to do so. We'll see if I decide to scratch it tonight.
