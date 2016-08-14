---
layout: post
title: TDD and Ontological Design
date: 2016-01-15
---

Imagine yourself in a time and place without any sort of societal
structure. It's the wilds, every man for himself, a *[state of
nature](https://en.wikipedia.org/wiki/State_of_nature), *chaos. Now
imagine that you and a couple of buddies decide that you'll fare better
in this *bellum omnium contra omnes* if you band together and work as a
team. Through your adventures you gather more and more people that agree
if with your way of thinking and doing things. The years go on and
eventually you have enough members in your group to create a country.
Where the country is simply the land you've decided to settle and the
shared values that keeps people there. (See what you've done? You've
created something by imposing constraints on what was otherwise chaos.)

One day a citizen of your country does something that's not at all ok
with all the other citizens. What happens? Well this person is refusing
to operate based on the constraints of the country, so in lieu of a jail
system (your country is still rudimentary) you either exile them or
eliminate them because they are dangerous to your system. fast-forward
several centuries. Your country is now a mighty empire with many states.
Each of these states are different from the others so they have their
own laws and societal norms that keep their mini-systems distinct and
operational. Within those states there might be counties and within them
cities on and on all the way down. The point I want to impress, is that
each of these imposed constraints directly effects the behavior of the
systems below/within it and in turn the people who choose to live in
those varying strata of society.

A while ago I came across the concept of [Ontological
Design](https://www.youtube.com/watch?v=aigR2UU4R20), or the idea that
what we design, designs us back. In other words, "We become what we
behold". When we design something whether it be a chair, a university
course, or a piece of software we impose a series of constraints on what
would otherwise be chaos. That's what creation is, structure from
nothing. However, as a result from creating those constraints we our now
forced to operate within them. Our creation is now modifying us to act
within its bounds, and the more constraints we impose on it, the more
and more distinct and specialized it becomes. The same is true of
software and specifically Test Driven Development (TDD).

At it's root, TDD is a feedback loop that ultimately results in the
completion of some sort of software. You start with Acceptance tests
which using our earlier analogy might be thought of as the laws that
help form a country and set it apart from everything else. Everything
follows from the acceptance tests. if something doesn't pass them it's
exiled/eliminated no matter where it lives within the system. Acceptance
tests force us to design unit tests that conform, and in turn, code that
conforms to the unit tests. It's called *intentional programming*. At
each step the more code we write the more what we write next depends on
what's already been written. We are forced to consider the system as a
whole at each step of the way, which I would argue leads to a higher
quality product. However, this is only possible as a result of the built
in feedback loops and this is what separates ontological design - which
I'd venture to say is good and useful - and poor design.

Consider for a moment, the alternative of ontological design. Poor
design, is something created without regard to its Quality (if I may
borrow from
[ZMM](http://www.amazon.com/Zen-Art-Motorcycle-Maintenance-Inquiry/dp/0060589469)).
There are no tests in place to determine it's merit (in a design sense),
it's wild and free. It's art for lack of a better term, not craft. As
such, it's governed and judged differently. Code can be art, but it
satisfies different purposes. All crafts, technologies, things created
with ontological design are at their root created for a singular
purpose: to modify the behaviors of the individuals that they are
intended to effect in a specific way. Without this intent, it exists in
a different category.

Getting back to TDD. TDD is good design, it considers this intent and
creates feedback loops to consistently determine whether it is on track,
it might not be the most pragmatic methodology out there to create
desirable software (read [TDD is
dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)
by the creator of Ruby on Rails), but to my knowledge it is the truest.

. . .

Aside from thinking about the above. I continued to practice typing
today making it all the way through the beginner courses. I intend on
repeating these until my speed and accuracy get up to desirable levels.
It might take a while. In addition, I got my first test to pass for
[clojure\_ttt.](https://github.com/Jgoodrich07/clojure-ttt) I'm pretty
comfortable with the [testing
framework](https://en.wikipedia.org/wiki/Test_automation)
[Speclj](http://speclj.com/)since it very closely resembles
[RSpec](http://rspec.info/),which I used for my ruby implementation of
Tic Tac Toe, and I'm becoming increasingly more comfortable with Clojure
itself as a language. I know one test isn't much but I'm feeling
confident and have a good idea of what to do next.

Till tomorrow!

 

 

 

 
