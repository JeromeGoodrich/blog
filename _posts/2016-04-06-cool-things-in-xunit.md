---
layout: post
title: Cool Things In xUnit
date: 2016-04-06
---

So far, I really appreciate and enjoy practicing the prime factors kata
and the idea of practicing katas in general. I mentioned in my post
yesterday that the opportunity to focus wholeheartedly on the
fundamentals was beneficial me to in a myriad of ways. That feeling
persisted today as I experimented with different ways to complete the
kata in order to decrease the time it takes me to complete it.

Apart from gaining proficiency with the VirtualStudio keyboard shortcuts
and my own touch typing, through discussion with another apprentice I
decided to try and use a C# dictionary in an attempt to reduce
duplication and improve speed. However, despite the novelty of this tiny
innovation, I wasn't able to gain much of an advantage. It still
required too much set-up and it decreased the readability of the code.

Given that I couldn't come up with much else in terms of optimization, I
became quite fixated on the idea of having one test that iterated
through a variable amount of test cases. The dictionary wasn't doing so
I consulted Myles to see if there was another way. As is often the case
in programming, there was.

Myles introduced me to some of the benefits of xUnit a testing tool for
the .NET framework, as well as some very helpful syntactic sugar in C
and a glimpse of LINQ (I'm sure this is an impending post). Anyway, in
xUnit uses two types of attributes when creating unit tests [Fact]
and [Theory], I really love their reasoning:

> Facts are tests which are always true. They test invariant
> conditions.*
>
> Theories are tests which are only true for a particular set of
> data.*

So as you might suspect, a theory implies variable data. xUnit has a
pretty cool way to handle this with the use of of the [InlineData]
attribute. Here's an example: the finished product of my tests after
completing the prime factors kata.

```c#
[Theory]
[InlineData (new [] {}, 1)]
[InlineData (new [] {2}, 2)]
[InlineData (new [] {3}, 3)]
[InlineData (new [] {2,2}, 4)]
[InlineData (new [] {2,3}, 6)]
[InlineData (new [ {2,2,2}, 8)]
[InlineData (new [] {3,3}, 9)]
public void TestPrimeFactorsGenerator(List<int> expectedPrimes, int num) {
  Assert.Equal(expectedPrimes, PrimeFactors.Generate(num));
}
```

One caveat regarding the use of the [InlineData] attribute: it only
works with system defined types. With this new implementation I'm
hovering around a 10 minute performance with several more rehearsals I
hope to get the entire performance somewhere between 5 and 10.
