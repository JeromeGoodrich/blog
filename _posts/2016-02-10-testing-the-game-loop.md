---
layout: post
title: Testing The Game Loop
date: 2016-02-10
---

Here's what I'm stuck on. I'm hoping that writing it out will give me
some kind of insight. I have a function, restart-game-maybe that
restarts a game if given the correct input from a user.

```clojure
(defn restart-game-maybe [game board markers io players]
  (let [response (get-input io)]
    (if (not= response "y")
      (exit 0 "See you next time!")
      (let [new-game (game-loop board players markers io)]
        (restart-game-maybe new-game board markers io players)))))
```

restart-game-maybe is a loop in that it calls itself, which makes it a
little difficult to test. Usually the function game-loop will go through
it's own loop until the game is over at which point it will return the
game end's board to restart-game-maybe. In order to determine whether or
not restart-game-maybe function actually calls itself a couple of things
need to happen. First, we need to a way to exit the loop, and second we
need to see if the function game-loop gets called. Let's tackle the
second requirement first.

Normally, if we call the function game-loop it will loop around until
that final board is reached. We don't care about that for testing. What
we do care about is knowing whether or not the function is actually
called. Clojure provides with-redefs to help out a bit. with-redefs
basically allows us to redefine a function. This is particularly helpful
in mocking out functions for testing. Mocking a function entails
simulating it to give a certain behavior so that it's easier to test.

```clojure
(defn exit [status message]
  (println status)
  (System/exit status))

(mock-exit [status message] status)
```

Since we usually only care about whether exit is being called and not
whether System/exit is actually exiting the program, it suffices to see
if exit is being called with the correct status. By using with-redefs we
can substitute a call to exit with mock-exit and can test against the
correct status message being returned. The test below passes.

```clojure
(describe "restart-game-maybe"
  (it "correctly exits the game"
    (with-redefs [exit mock-exit]
      (let [game ["X" "O" "X" "O" "X" "O" "X" 7 "O"]
            board [0 1 2 3 4 5 6 7 8]
            markers ["X" "O"]
            io (new-test-console "n")
            players [(new-human-player io) (new-computer-player 3)]]
      (should= 0 (restart-game-maybe game board markers io players)))))
```

Back to the problem at hand: We want to know whether game-loop is being
called but don't care about what it does once it is called. It's an
ideal candidate for a with-redefs mocking. What if we did the following?

```clojure
(def game-loops (atom 0))

(defn mock-game-loop [& args]
  (swap! game-loops inc))
```

Now every time we call mock-game-loop the value of game-loops will
increase by 1. we can now run the restart-game-maybe function in our
test with game-loop mocked out for mock-game-loop and use (deref
game-loops) to see that the mock-game-loop is indeed getting called. One
down, one to go. We still need some way to get out of the
restart-game-maybe loop.

The easiest way to do this is to have response not equal "y". Let's look
at what a test for this might look like.

```clojure
(it "starts a new game with the same config"
  (with-redefs [exit mock-exit
                game-loop mock-game-loop]
    (let [game ["X" "O" "X" "O" "X" "O" "X" 7 "O"]
          board [0 1 2 3 4 5 6 7 8]
          markers ["X" "O"]
          io (new-test-console ["y" "n"])
          players [(new-human-player io) (new-computer-player 3)]
          restart-game (restart-game-maybe game board markers io players)]
    (should= 1 @game-loops)))))
```

Our current problem deals with how our binding of io to
`(new-test-console ["y" "n"])` the idea is that after we loop through
restart-game-maybe once we exit the loop by providing an input of "n"
instead of "y". How do we do this? If we keep things as is, the input
will be ["y" "n"], and thus we won't even loop through once because
the only condition we have to satisfy in order to exit is for the input
to not be "y". We know a way to access the stuff inside the sequence one
at a time, we also know that accessing each piece does not occur at the
same time, this rules out things like map.

Let's take a closer look at the the new-test-console function and the
TestConsole type it creates. I have a hunch this is where the magic
needs to happen.

```clojure
(deftype TestConsole [input]
  Presenter
    (print-io [this something])
    (display-end-result [this result markers])
    (get-input [this]
      input)
    (display-board [this board])
    (prompt [this something]
      (get-input this)))

(defn new-test-console [input]
  (TestConsole. input)
```

There's a lot going on here, but the only thing we really need to focus
on is the new-test-console function and the get-input function. In this
current implementation our input can be anything and whatever it
get-input will just return it as is. Clearly something has to change. We
have two potential options. Primo, We can try to access different parts
of the vector based on whether or not we've looped through the
restart-game-maybe function. Secondo, we can change manipulate the input
so that it returns what we want.

While you could probably do it the first way, it seems like it would
introduce unnecessary complexity and coupling in the code. Here's how we
implement the second option and successfully test that
restart-game-maybe actually restarts the game when given the right
input.

```clojure
(deftype TestConsole [input]
  Presenter
    (print-io [this something])
    (display-end-result [this result markers])
    (get-input [this]
      (let [return-this (first @input)]
        (do (swap! input rest)
          return-this)))
    (display-board [this board])
    (prompt [this something]
      (get-input this)))

(defn new-test-console [input]
  (TestConsole. (atom input)))
```

What's going on here? When we call new-test-console we provide it with
an input. that input get's converted into an atom, By doing so, we
create an object whose value we can manipulate to suit our needs. When
get-input is called we want to return the first value in the vector --
that is now the value of the input atom -- so we use @/deref to access
the value. from there we alter the value of the input atom by converting
it to a lazy sequence that eliminates the first value of the initial
sequence and returns the rest. Now if we call get-input again we will
return the next value in the initial sequence. Problem solved and
behavior tested.
