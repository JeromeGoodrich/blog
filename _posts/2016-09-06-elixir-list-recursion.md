---
layout: post
title: Elixir - List Recursion
date: 2016-09-06
---


The first programming book I ever read was "Learn to Program" by Chris Pine. I remember making my first couple of interactive
programs, and thinking, "Ok, this programming thing isn't too bad", and then about midway through the book. I got to the chapter
on recursion and pretty much hit a wall. Recursion is a difficult concept to wrap your head around, and I consider it a 
rite of passage when I finally understand it in a new language.

In Elixir, recursion is usually explained along with lists, because they tend to work very nicely together. To get a better feel
for recursion and lists I tried my hand at creating my own implementations of Elixir's `Enum` module and so I think that might
be a good way to explain it below as well. Let's dive in.

### Reduce

Map, Filter, and Reduce are some of the most important functions in programming many langauges. We won't get into Map and Filter
but the majority of Enumeration functions in many languages can be implemented using Reduce. The concept of reduce is simple.
It takes a collection and, wait for it, _reduces_ it down to one value. The thing that we want to explore is how this happens 
recursively in lists. 

Before jumping right into building our own reduce function lets start slow and first create something that can get the sum of
all of the elements in the following list: `[1, 3, 5, 9, 10]`. As we will see, implementing sum will naturally lead us to an
implementation of reduce. We'll start with a test of the simplest condition, an empty list.

```elixir

defmodule RecursionExampleTest do
  use ExUnit.Case
  import RecursionExample
  
  test "the sum of an empty list is 0" do 
    assert sum([]) == 0
  end
end
```

We now write the simplest thing to make our test pass.

```elixir

defmodule RecursionExample do
  
  def sum(list) do
    0
  end
end
```

The second test, simply asserts that the sum of a list with one number is that number.

```elixir

defmodule RecursionExampleTest do
  use ExUnit.Case
  import RecursionExample
  
  test "the sum of an empty list is 0" do 
    assert sum([]) == 0
  end
  
  test "the sum of a list with one number is that number" do 
    assert sum([5]) == 5
  end
end
```

This too, is easy to pass. We just write the simplest thing, and since Elixir supports the same function with mutiple bodies,
we can just define another sum function that takes and returns what we need.

```elixir

defmodule RecursionExample do
  
  def sum([]), do: 0
  def sum([n]), do: n 
end
```

As you might expect, the next test adds another number to our list, and asserst that the sum of those two numbers is the sum
of the list. 

```elixir

defmodule RecursionExampleTest do
  use ExUnit.Case
  import RecursionExample
  
  test "the sum of an empty list is 0" do 
    assert sum([]) == 0
  end
  
  test "the sum of a list with one number is that number" do 
    assert sum([5]) == 5
  end

  test "the sum of a list with one number is that number" do 
    assert sum([5, 5]) == 10
  end
end

```

The solution to this, is again writing another simple sum function with that takes and returns what we want. However,
We can start to see that this approach will start to get cumbersome if our list grows any bigger.

```elixir

defmodule RecursionExample do
  
  def sum([]), do: 0
  def sum([n]), do: n 
  def sum([x,y]), do: x + y
end 
```
Our next test, tests what we actually want to test, and will force us to reconsider our design.

```elixir

defmodule RecursionExampleTest do
  use ExUnit.Case
  import RecursionExample
  
  test "the sum of an empty list is 0" do 
    assert sum([]) == 0
  end
  
  test "the sum of a list with one number is that number" do 
    assert sum([5]) == 5
  end

  test "the sum of a list with one number is that number" do 
    assert sum([5, 5]) == 10
    assert sum([1, 3, 5, 9, 10]) == 28
  end
end

```
Let's start thinking about things in terms of lists. Lists can either be empty or have a head and tail which we can represent 
like so: `list = [head | tail]`. The tail of a list, is a list itself. So a list of 1 element like `list = [ 1 ]` can actaully
be written as `list = [ 1 | [] ]`. Pretty neat right? Now, if we think about our `sum` function in these terms we can realize
what it is actually doing. It's taking the head of the list, putting it somewhere, and calling `sum` again with the tail of the
original list and adding that new head to the previous head. It does this all the way until the list is empty, at which point it
will return whatever the total is. That's a lot to chew on. So let's look at it in code.

```elixir

defmodule RecursionExample do

  def sum([], total), do: total
  def sum([head | tail], total) do
    sum(tail, head+total)
  end
end

```
Wait, if we run this, our tests will still broken because our new sum functions take 2 parameters: a list and a total instead of
just a list. Instead of changing all of our tests, we can simply refactor to something like this: 

```elixir

defmodule RecursionExample do

  def sum(list), do: _sum(list, 0)

  defp _sum([], total), do: total
  defp _sum([h | t], total) do: _sum(t, h+total)
end
```
This is a pretty common pattern in Elixir. Define private helper methods (prefixed either with `_` or `do_`) that are invoked
by a public function. The prefix isn't necessary it just makes that relationship explicit. 

With that, we have finished created our generic `sum` function. The next thing we might realize is that all sum, does is take
a collection, and return a single value - the sum of that collection. That's exactly what reduce does! So let's write some tests
for our reduce function in each of the test cases we used for our sum function. After all, they should be able to do the same thing.

Our sum function took a list and a accumulator, which we called `total` our reduce function will need to take the same things
except it will also need a sum function that we can pass to it. Like our sum function this function will need to have 2 parameters
a list and an accumulator. Our reduce function starts to look like this

```elixir

reduce([], acc, _), do: acc
reduce([head| tail], acc, function) do
  reduce(tail, function.(head, acc), function)
end
```

The accumulator is just whatever is returned when we call the function with the head of the current list and current value of
the accumulator. To implement `sum` we just need to add things together so we can pass in the anonymous function 
`fn(x, y) -> x + y end` which we can shorten using some syntactic sugar to `&(&1 + &2)`. Thus we can now write out our test as:

```elixir

defmodule RecursionExampleTest do
  use ExUnit.Case
  import RecursionExample
  
  test "the sum of an empty list is 0" do 
    assert sum([]) == 0
    assert reduce([], 0, &(&1 + &2) == 0
  end
  
  test "the sum of a list with one number is that number" do 
    assert sum([5]) == 5
    assert reduce([5], 0, &(&1 + &2)) == 5
  end

  test "the sum of a list with one number is that number" do 
    assert sum([5, 5]) == 10
    assert sum([1, 3, 5, 9, 10]) == 28
    assert reduce([1, 3, 5, 9, 10], 0, &(&1 + &2)) == 28
  end
end

```
And finally our reduce function as:
``` elixir 

defmodule RecursionExample do
  
  def reduce([], acc, _), do: value
  def reduce([h | t], acc, func) do
    reduce(tail, func.(head, acc), func)
  end
  
  def sum(list), do: _sum(list, 0)

  defp _sum([], total), do: total
  defp _sum([h | t], total) do: _sum(t, h+total)
```
