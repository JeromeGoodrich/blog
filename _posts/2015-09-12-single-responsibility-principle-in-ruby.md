---
layout: post
title: Single Responsibility Principle in Ruby
date: 2015-09-12
---

The poor classes in my TicTacToe game are over-worked and confused. They
are overachievers trying to do too many things at once and adding
unnecessary complexity to my program as a result. The program works, it
ekes by like a understaffed restaurant on a busy night, but it's just
barely hanging in there and really needs some organizational changes if
it's going to continue.

In Ruby, it's often said that each class should have one and *only* one
responsibility. This principle is aptly named the **single
responsibility principle** and it's crucial for a well designed program.
If a ruby class only has one responsibility it's much easier to test,
and change in the event a new feature is added or something like that.
Segregated responsibilities also just make the code more readable and
easy to follow. There's obviously a limit a code that's overly
deconstructed can become overly complex, so as [this
article](http://jjbohn.info/blog/2014/07/28/single-responsibility-principle-a-solid-week/)
suggests another definition might provide a more actionable heuristic
then simply *"a ruby class should only have one responsibility"*.  The
article settles on *“Design classes so there should never be more than
one reason for a class to change.”* as it's working definition for the
the single responsibility principle. I like it because it encourages a
rubyists to consider how their code might change, which is something
that I wish I had in the back of mind as I was working on the 2nd
iteration of the TicTacToe game.
