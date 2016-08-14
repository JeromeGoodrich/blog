---
layout: post
title: Getting Back On Track
date: 2016-02-01
---

I made some good strides to get back on track today. Did most of the
things I normally do and feel much better for it. Today, I officially
finished the first real "story" of my apprenticeship: making a computer
player that wins 85% of the time, along with a test that proves it.

I way underestimated my ability to get it done (actual: 3.5 pts
expected: 7.75 pts), but It still took longer than I thought. I
initially thought I had finished in the morning, since the tests worked,
but the requirements were actually to have the new AI playable through
the command line interface. This required a lot more work than I
initially thought and revealed to me how brittle my code was.

I had to change a lot, and eventually settled on creating a HumanPlayer
type and ComputerPlayer type that both used the Player protocol, which
handled the make- move function. This allowed me to get rid of my former
make-move function, make my game-loop even more concise, add new options
to the cli, and gave me a good opportunity to refactor my -main
function.

Last week, Kevin suggested giving myself a stopping time since it's easy
to get wrapped up in a problem and end up working late into the night. I
stopped coding at 4PM, and read two chapters of PPP before blogging.
I've gone through 4 of the 5 SOLID principles and am looking for another
source to shore up some of the ambiguities I still have about it, since
it's talked about pretty technically, and in my opinion, opaquely in
PPP.

I'm about out of steam. Hopefully, I'll be a bit fresher for my next
post.
