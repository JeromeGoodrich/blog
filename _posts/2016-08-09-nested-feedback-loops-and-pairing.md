--
layout: post
title: Nested Feedback Loops and Pairing
date: 2016-08-09
---

Today I learned about and implemented what's referrred to in "Growing Object-Oriented Software, Guided by Tests" as
'nested feedback loops'. In the context of TDD, a 'nested feedback loop', is a top down approach that suggests writing
a failing functional/integration test to reveal a unit test that could be written. Once the unit test is passed, the next
part of the integration test fails, revaling another unit tests that could be written, and so on until the entire
functional/integration is passed.

This was the primary pattern that Eric M. and I employed today while refactoring an app's UX in coffeescript. We would write
a test that described the overall behavior we'd expect from a user's action. For instance, a test that describes a user updating
a form, would not only update that form it would query the database for a corresponding date the user-input is associated with while
potentially hidding another input field. This complex UX behavior would then be tested more granularly through a suite of unit tests
which would in turn get us closer and closer to passing our integration test. Once we did we would have the condfidence to know that
part of the codebase was well covered.

While working with Eric, through this particular flavor of TDD, I was struck by several differences in our approaches. Eric
never seemed to miss anything. Or more accurately, it seemed like he was extremely careful about making assumptions. Any
assumption that he did make, was immediately tested or there was already a test in place to corroboarate the assumption. As
as result, It seemed that Eric hardly made any mistakes. Sure there were plenty of invalid assumptions, but they never caused
any problems. For example, instead of deleting something. We always first commented it out so it was around until we were absolutely
sure it wasn't needed anymore.

We were also constantly referring to the git, verifying any changes we've made and getting a more comprehensive view of what
we had actually done. When we did commit it was incremental with regular amendments, and a lot of git stashing and popping to
toggle between states. It's been really edifying. I'm a little frustrated I haven't been more awake. I feel like I could be
getting even more out of the experience.
