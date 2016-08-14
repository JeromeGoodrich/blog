---
layout: post
title: Formatting The Board
date: 2016-02-02
---

Yeah, it's a long road...

The majority of the day was spent jotting down pseudo code and diagrams
in my notebook, with an occasional visit to the REPL to verify a few
things. I started off with the idea that I could simply match up the
string version of an empty tic tac toe board and replace certain
characters in it based on their index within the string. I didn't figure
out a way to do this, although I'd be surprised if it was anymore
complex than what I ended up with. I reasoned that there were 4 space
strings that I needed to concern myself with "___|" "___" "    
 |" and "        ". Arrange a combination of those 4 in the right order
and you've got a board. Each of these spaces have two potential states,
empty or filled. This means that in total there are 8 potential states
to account for.

With this in the back of my mind, I first decided that the first thing I
would need to do is split up the board into units that correspond to the
different types of spaces. For example, given that the board is just a
vector 0 1 2 3 4 5 6 7 8], I would want the spaces [0 1 3 4] to
some how be converted into "__|" or "_X_|" if there was a marker
in one of them. I decided to split the board up into rows and then split
the rows up further so that I could map the correct spaces to the
correct string representations of the board. This eventually, worked,
but despite my efforts there appears to be some needless repetition as
well as some potential dependency issues, which I hope to refactor and
clean-up tomorrow. I think should mention that I have not been using TDD
at all during these "stories". I'm aware that my code and specifically
my UI needs to be refactored to make the code more easily testable, and
since I haven't done that yet I'm not quite sure how to get better test
coverage. Perhaps, that should have been the first thing I did...

I finished reading about the final SOLID principal today, and I feel
like I need to go back and revisit all of them in a different context.
They aren't quit sticking for whatever reason.

A couple of other things I've noticed:

-   I've completely fallen off the wagon with typing practice
-   The Vim shortcuts tab in my browser is consistently open, but
    seldom visited.

 
