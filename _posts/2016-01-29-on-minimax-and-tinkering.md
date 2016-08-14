---
layout: post
title: On minimax and tinkering
date: 2016-01-29
---

Today I want to talk about tinkering, the next step in the natural
progression that begins with stuckness, which I wrote about a little
while ago. Tinkering is how we get unstuck. It's the process of trial
and error that eventually leads us down the path to progress.

I learned a lot about tinkering while trying to build an unbeatable
computer player my Clojure TicTacToe, using the "minimax" algorithm. To
start, I had some fundamental knowledge of how the algorithm worked from
when I had to create it in my Ruby version of TicTacToe, and from that
experience and the feedback I got from it, I should have remembered that
the best thing for me to do was write out in plain english what it was
that I needed my code to do, and then convert that into pseudo code and
start building from there.

I would call this post-hoc realization my first rule of tinkering:

**RULE 1: Start with low fidelity.**

In other words, don't invest time and mental energy into writing code
when you have no clue what that code is supposed to be doing. Here's
what I wish I had wrote before I even started coding:

1.  For a given board find all the unmarked spaces on that board
2.  For each unmarked space, create a new board and fill in that space
    with the cpu's marker
3.  Evaluate each board for a terminal state (win, loss, tie)
4.  if a board is in a terminal state, associate a score (win+10 loss
    -10 tie 0) with the space
5.  if not in a terminal state repeat steps 1-4 but filling in the
    spaces with the opponents marker instead. alternate the markers with
    each repetition
6.  find the max of all terminal states reached when it was the cpu's
    turn and the min when it was the opponents turn. associate these
    scores with the initial-marked space
7.  Find the max of all the scores that are returned and select the
    space associated with the max score.

It's not pretty, it's not entirely accurate and it's missing some
information, but it was much quicker and easier then trying to figure
this all out in code, from scratch (at least, for me). It gives me a
pretty good idea of what some of my dependencies will be, where I might
need to use a recursive function, and what kind of data-structures to
potentially use. More than enough to make some serious progress in
pseudo-code.

```clojure
;minimax [board markers]
;unmarked-spaces = (filter number? board)
;create new board for each unmarked space =
;(map #(mark-spot % marker) unmarked-spaces)
;score each move = (map #(get-score %1 %2 markers) unmarked-spaces possible-boards)
```


Sweet. So writing out the pseudo-code based on my lo-fi instructions is
helping me give some shape to this algorithm early on. I now have this
notion of a get-score function that seems to be the meat of my algorithm
and deserves it's own pseudo-code treatment.

```clojure

;I'm mapping over the get score function so the parameters are
;individual spaces and boards.

(get-score [space board markers]
;first evaluate scores for different game-states
  (cond
    (win-game? board (first markers)) (assoc space {:score 10})
    (win-game? board (second markers))(assoc space {:score -10})
    (tie-game? board) (assoc space {:score 0})
    :else reverse player order = (reverse markers)

; unmarked-spaces = (same as before)
; create new board for each unmarked space = (same as before)
; score new boards = (same as before but we want the same space
;for each board since we only care about the first move)
; if it was your turn last
; (max-by-score scored boards) - we want to return the space with the best
;score since that's the one the computer will
; want to choose on their turn
; (min-by-score scored boards) -

;we want to return the space with the worst
;score since that's the one that the opponent
;will choose on their turn if the game is perfectly played
```

Sweet, so if I had done this initially, I'd be in a pretty good spot.
Cutting this post short now since I'm way behind.
