---
layout: post
title: Tail Recursive Minimax & alpha-beta pruning
date: 2016-02-18
---

One of the many optimizations that can be made to the minimax algorithm
is the inclusion of what's called alpha-beta pruning. In tic-tac-toe,
given the current state of the game, Minimax typically works by
generating all possible subsequent moves for a player and based on those
game states, all possible moves until an end-state (tie, win, loss) is
reached at which point the board is "scored". Minimax then works it's
way back up the "tree" by comparing the scores of the nodes at each
level for each game-state. This [short
video](https://www.youtube.com/watch?v=zDskcx8FStA) provides a nice
visualization for what's happening.

Alpha-Beta pruning improves minimax by eliminating inconsequential
"branches" from the minimax search. With less to go through, the
algorithm is faster.

![AB-Pruning](%7B%7B%20site.baseurl%20%7D%7D/assets/ab-pruning.png)

After tussling with minimax, I feel like I have it down pretty well. So
normally, I don't think implementing  alpha-beta pruning would take that
long.  I would need to switch my code up a bit, but I have a pretty
vivid image of the implementation in my mind. It's a different story
when one of the requirements is for the algorithm to also use tail
recursion.

> In [computer
> science](https://en.wikipedia.org/wiki/Computer_science "Computer science"),
> a **tail call** is a
> [subroutine](https://en.wikipedia.org/wiki/Subroutine "Subroutine")
> call performed as the final action of a procedure. If a tail call
> might lead to the same subroutine being called again later in the call
> chain, the subroutine is said to be **tail-recursive**, which is a
> special case of
> [recursion](https://en.wikipedia.org/wiki/Recursion_(computer_science) "Recursion (computer science)").
> **Tail recursion** (or **tail-end recursion**) is particularly useful,
> and often easy to handle in implementations
>
> -*Wikipedia (Tail call)*

We use tail-recursion in many functional programming languages like
Clojure because it doesn't add stack frames to the call stack.
Typically, when a function is called the computer must remember the
place in the code the function was called from, and this information is
stored in the call stack. For complex recursive functions where each
call to the function will add a stack frame to the calls stack, the call
stack can easily overflow. To get around this we use tail-recursion as
it consumes no stack space since the newly called function returns its
result to the original caller.

Anyway, tail recursion requires thinking about minimax a bit
differently, and specifically I found it helpful to think in terms of
the game tree.  In a TicTacToe game, the game tree starts from a root
node that corresponds to a particular game-state. All the potential
game-states that can result from the root nodes game-state are contained
within the children of the root node, these nodes are also called
internal nodes. Each internal node has children of it's own up until the
point that a terminal state is reached (the leaf node). The pseudo code
below attempts to establish some structure and intent around what
behavior we should expect from the minimax function:

```clojure
;if current-node = root and all children are scored get child with the
;best score. Return the space associated with that node.
;if current-node is an internal-node, all it's children are scored
;and maximizing player's turn get the max score of all it's children
;and not maximizing player's turn get the min score of all it's
;children
;if current-node is internal and depth = 0. Return the score of the
;node
;if current-node is leaf. Return the score of the node
;if current-node is internal and not all children are scored, score next
child.
```

In terms of the game tree, the minimax function is trying to get to a
node it can score so it can then compare those scores and select the
best one. In other words, it's generating new nodes while keeping track
of the old ones. At it's heart, that's all that's all the minimax
function does.

In Clojure tail recursion is implemented with the special
operator **recur.** recur works by accepting the same number of
arguments present in the initial function and changing any number of
them before returning them to original function.

```clojure
(defn compute-across [func elements value]
  (if (empty? elements)
    value
    (recur func (rest elements) (func value (first elements)))))
```

The key to implementing tail recursion is holding the state of what you
need to keep track of in the arguments of the original function. for
this reason, tail-recursive functions will often have more arguments
then their forward recursive counterparts. In the case, of minimax for
TicTacToe we need to keep track of nodes we have already "visited" and
all the information contained within those nodes. The fact that nodes
contain information about the game-state tells me that a node is some
sort of nested data structure. Furthermore, given that we each node
contains the same types of information with only the values of the
information getting changed tells me that one probable implementation of
a node could be a hashmap. However, I ended up getting adventurous and
using  Clojure's defrecord instead, which is an object but acts
similarly to a hashmap.

Here is my implementation of node:

```clojure
(defrecord Node [parent children board depth type markers score])
; board is a hashmap consisting of a game-state and the space the board
is space (which can be thought of as a branch ID)
```

To be fair, It took a bit of time to figure out what all the
information.  stored in a node would be. I had to look back at my
original minimax function and identify all the places where the call
stack was keeping state, and even then sometimes I had to manipulate
certain elements to have them make sense. Eventually, though, I was able
to refine the pseudo code to be a bit more explicit before writing
things out for real.

```clojure
;(defn minimax [node]
;   node-score = (get-score (:board node) (:markers node) (:depth node))
;   (if (not= nil node-score) or (= depth 0) or (empty? (:children node))
;     (let [score = (:score node) or node-score]
;   elsif root-node
;     get space associated with child that has best score
;   elsif (= (:type node) "max")
;     if (:score (:parent node)) = nil or score &amp;gt; (:score (:parent-node)
;       make (:score (:parent node)) = score
;     else go back up to parent to access next child
;   elsif (= (:type node) "min")
;     if (:score (:parent node)) = nil or score &amp;lt; (:score (:parent-node))
;       make (:score (:parent node)) = score
;     else go back up to parent to access next child
;   else generate the next node and visit it.
```


From the pseudo code, I think we start to have a pretty good idea of
where all our calls to recur might be. It's anywhere where we are
modifying the current or visiting another node. The only things left
then are implementation details which for me, typically means that it's
time to actually write out the code.


```clojure
(defn minimax [node]
  (println (:children node))
  (let [current-node-score (score-board (:state (:board node)) (:player-type node) (:depth node))]
    (if (or current-node-score (empty? (:children node)) (= 0 (:depth node)))
      (let [score (or current-node-score (:score node))]
        (if (nil? (:parent node))
          (:space (:board node))
        (if (= (:player-type node) "max")
          (if (or (nil? (:score (:parent node))) (> score (:score (:parent node))))
            (recur (-> (:parent node)
              (assoc-in [:score] score)
              (assoc-in [:board :space] (:space (:board node)))))
            (recur (:parent node)))
          (if (or (nil? (:score (:parent node))) (< score (:score (:parent node))))
            (recur (-> (:parent node)
              (assoc-in [:score] score)
              (assoc-in [:board :space] (:space (:board node)))))
            (recur (:parent node))))))
              (let [new-node (Node. (update-in node [:children] rest)
                                    (create-possible-boards
                                      (:state (first (:children node)))
                                      (:markers node))
                                    (first (:children node))
                                    (dec (:depth node))
                                    (if (= (:player-type node) "max")
                                      "min"
                                      "max")
                                    (reverse (:markers node))
                                    nil)]
                 (recur new-node)))))
```


I'd love to say I got figured this out on the first try, but of course
it took some debugging and several hours of frustration before getting
it to finally work. Having the tests from the original minimax function
definitely helped, but it was a lot of printing outputs to the console
and checking them against my expectations. Shout out to Jeff R. for his
patience and help as a rubber ducky.

With the tail recursive minimax working, all my tests ran in about 45
seconds, up from the 20 seconds they were running at with the original
function. This is no good and means that not only is alpha-beta pruning
preferential it's necessary.

I mentioned above that alpha-beta pruning works by eliminating
inconsequential branches from the minimax search, but what does that
actually mean? Let's unpack what's actually going on.

> The algorithm maintains two values, alpha and beta, which represent
> the maximum score that the maximizing player is assured of and the
> minimum score that the minimizing player is assured of respectively.
> Initially alpha is negative infinity and beta is positive infinity,
> i.e. both players start with their lowest possible score. It can
> happen that when choosing a certain branch of a certain node the
> minimum score that the minimizing player is assured of becomes less
> than the maximum score that the maximizing player is assured of
> (beta<=alpha). If this is the case, the parent node should not
> choose this node, because it will make the score for the parent node
> worse. Therefore, the other branches of the node do not have to be
> explored.
>
> -*Wikipedia (Alpha-Beta Pruning)*

This tells us 3 things in relation to our current algorithm.

1.  We need to update alpha to = the current max-score of the maximizing
    player
2.  We need to update beta to = the current min-score of the minimizing
    player
3.  We need to not generate any new nodes if beta <= alpha

Looking at the code it's easy to see where these things would happen,
and were left with our final result:

```clojure
(def minimax (memoize (fn [node]
  (let [current-node-score (score-board (:state (:board node))
                           (:player-type node) (:depth node))]
    (if (or current-node-score
            (empty? (:children node))
            (= 0 (:depth node))
            (<= (:beta node) (:alpha node)))
      (let [score (or current-node-score (:score node))]
        (if (nil? (:parent node))
          (:space (:board node))
          (if (= (:player-type node) "max")
            (if (or (nil? (:score (:parent node)))
                    (> score (:score (:parent node))))
              (recur (-> (:parent node)
                         (assoc-in [:score] score)
                         (assoc-in [:alpha] (max score (:alpha (:parent node))))
                         (assoc-in [:board :space] (:space (:board node)))))
              (recur (:parent node)))
            (if (or (nil? (:score (:parent node)))
                    (< score (:score (:parent node))))
              (recur (-> (:parent node)
                         (assoc-in [:score] score)
                         (assoc-in [:beta] (min score (:beta (:parent node))))
                         (assoc-in [:board :space] (:space (:board node)))))
              (recur (:parent node))))))
       (let [new-node (Node. (update-in node [:children] rest)
                             (create-possible-boards (:state (first (:children node)))
                                                     (reverse (:markers node))
                                                     (:space (first (:children node))))
                             (first (:children node))
                             (:alpha node)
                             (:beta node)
                             (dec (:depth node))
                             (if (= (:player-type node) "max")
                                "min"
                                "max")
                             (reverse (:markers node))
                             nil)]
          (recur new-node)))))))
```

One last thing. I used Clojure's memoize function as a way to speed up
the code even more. Memoize caches the result of a function call so that
when the same inputs occur again it returns the cached result instead of
calling the function again, for functions when calls with same arguments
are often repeated, there is better performance, but at the cost of
higher memory use.

**Closing Thoughts**

I spent the better part of a week figuring out all aspects of this
problem, and as such fell behind a bit in writing these posts. I think
the length and detail in this post somewhat atones for the lack of
consistency as it is pretty much a summary of my thinking over the past
week. Parts of me really like these longer form posts. Even though they
take up more time and it's unrealistic that I'm able to write one
everyday, I'm able to let my thoughts about things marinade and I feel
like the quality is much higher.
