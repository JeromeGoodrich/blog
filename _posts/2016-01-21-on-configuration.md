---
layout: post
title: Learning Words - Configuration
date: 2016-01-21
---

I'm easily intimidated by code. Often I feel like I'm peering through a
dark glass. If I squint I can barely make out shady figures on the other
side, but I don't understand their intentions. Are they friends or
enemies?  90% of the time I mistakenly choose the latter. Every so
often, when the light hits just right, I can clearly identify one of
these figures and recruit them to my cause. Today I want to talk about
one of these rare instances. Today I want to talk about *configuration*.

> Configuration - the way the parts of something are arranged

It's a word I knew before I started coding and it appears all over the
place (usually as "config") when developing software. Yet, until today,
it never really dawned on me why. I understood that most things needed
some sort of configuration, but I took it at face value.

During my student apprenticeship I had to build a to-do list app that
could work online as well as in in the command line. I also had to make
sure that the app had tests for both interfaces. The code below is from
a file in my todo app called spec_helper.rb

```ruby
require 'rack/test'
require 'rspec

require File.expand_path "../../sinatra_todo_app.rb", __FILE__

ENV['RACK_ENV'] = 'test'

module RSpecMixin
  include Rack::Test::Methods

  def app()
    Sinatra::Application
  end
end

RSpec.configure do |config|
  config.include RSpecMixin
  DataMapper::setup(:default, "sqlite3://#{Dir.pwd}/list_test.db")
  DataMapper.finalize

  List.auto_migrate!
  Task.auto_migrate!
  User.auto_migrate!
end
```


As I remember, I think I found a version of this code on someone else's
blog and hacked away at it until it did what I needed, only (vaguely)
understanding after the fact what it was actually doing. Surprise, it's
a configuration file! See that

`Rspec.configure do |config|`

That's the giveaway. Basically, all that code before are the parts we
need to configure the coding library RSpec to do what we want. All that

 `Rspec.configure do |config|`

is doing is taking those parts and arranging them in the way we want.

Again, when I first did this, my understanding was a bit more
rudimentary. It was more like, "I know that this stuff is necessary for
me to do what I want in another file." Almost there! Unfortunately, it
would take another 2 months or so for it to sink in and in the meantime,
I'd walk around thinking that I had a firm grasp of the concept, and why
shouldn't I, my code worked.

Today, I was pairing with Kevin on my TicTacToe game in Clojure. I was
trying to understand how I could separate the game logic - code that
allowed calculated what move to make, who's turn it was, etc -from the
user interface - things like asking the players what their names were,
what marker they would like to do, etc. below is my create-players
function, which, you guessed it! creates players and exemplifies the
issue I'm having with separating the game logic from the user interface.

```clojure
(defn create-players [number]
(cond
(= "1" number) (let[players [{:name (prompt "What is your name?")
:marker (prompt "Select a marker (it can be any letter)")}
{:name "TicTacJoe" :marker (prompt "Select a marker for the
computer")}]
order (prompt "Who will go first? (me/comp)")]
(cond
(= order "me") players
(= order "comp") (reverse players)))

(= "2" number) (let [players [{:name (prompt "Player 1, what is your
name?")
:marker (prompt "Select a marker (it can be any letter)")}
{:name (prompt "Player 2, what is your name?")
:marker (prompt "Select a marker (it can be any letter not chosen by
player 1)")}]] players)))

[/code]

So if we called this function - which means that you make the function
do what it was created to do - expecting 2 humans to play each
other, then we would expect an output like the following:

[code language="clojure"]

[{:name "Jerome" :marker "X"} {:name "Sol" :marker "O"}]

[/code]

We've created a vector that houses two hash maps. The first hash map we
can consider to be for player 1. The player's name is Jerome and for
this game of Tic Tac Toe he will using the X marker. We infer the
similarly about player 2 using the second hash map. The question is how
did we get these values?  Do you see all those places in the code where
it says "prompt"? That's how.

[code language="clojure"]
(defn prompt [message]
(println message)
(read-line))
[/code]

prompt is a simple function that will print what I type after it and
return what the user inputs in response. It's an example of user
interface code. It has nothing to do with how the game is run it just
gets input from the user and then leaves it up to the game logic to do
something with it. The problem is that it doesn't belong in a function
like create-players since create-players is a game logic function. So
how do we solve this?

Configuration, of course! Let me explain. We know that configuring
something means that we arrange a bunch of parts to do something that we
want. The parts in this case are the inputs from the users that we get
from a multitude of possible locations. In this particular instance
there are two we care about:

1.  user interface functions like prompt
2.  command line arguments and options parsing (all this means is that
    when we fire up the TicTacToe from the command line we can add
    things like -p1 human -p2 computer to specify what type of player,
    the order of play, etc.) Clojure has a useful tool help with this
    that I am using.

Regardless, of where we get the data, it can't come in raw, our game
logic functions are lazy and want all the work done for them. it's
excusable, they are doing a lot of other work, they shouldn't have to
alter the data so they can use it better too. So we create some new
functions that do the work of configuring the raw data into something
usable by the game logic functions.

*I haven't written those functions yet, but I may add them at a later
date*

.  .  .

Writing it out now, the concept of configuration seems so elementary,
but I think it's those basic fundamentals that are actually the hardest
things to fully grasp. Because they seem so obvious I tend to take them
for granted and they cause me a lot of needless frustration in the
meantime. I think it get's back to something I talked about in [my post
on being
stuck](https://jeromegoodrich.wordpress.com/2016/01/19/the-long-road-day-6/).
Check the ego at the door, I'm only going to learn something if I admit
to myself that I don't know it, and as a result the code will benefit as
well.

 
