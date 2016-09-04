---
layout: post
title: Elixir - Import, Alias, and Require
date: 2016-09-02
---


In Elixir, `modules` provide names for things you define. If you want to reference something defined in a module outside of it you just
prepend the module name to the beginning.

```elixir

defmodule MyModule do
  def do_something do
    IO.puts "What do you want me to do?"
  end
end

def AnotherModule do
  def do_something_again do
    MyModule.do_something
  end
end

```

But let's say you want to use something in a Module that comes from somewhere else. Elixir provides three _directives_ to help you out.

### Import

You know how we called a function outside of it's module by prepending it's Module name? Well if we don't want to do that anymore we
can just use the `import` directive.


```elixir

defmodule MyModule do
  def do_something do
    IO.puts "What do you want me to do?"
  end
end

def AnotherModule do
  def do_something_again do
  import MyModule, only: [do_something: 0]
    do_something
  end
end

```

 `import` has an optional second parameter that allows you control which functions or macros are actually imported. The supported options
are `only:` or `except:` followed by a list of `name: arity` pairs (the term arity just refers to the number of arguments a function takes).
So if you use `only:` you will only import the functions that are specified, and if you use `except:` the opposite will be true - you'll get
everything in the module _except_ for the ones that are specified. Typically, you'll want to use import in the smallest possible enclosing scope...

Yeah... when I read something like that it didn't really mean all that much to me either. Here's what it means (apologies for the digression)

`import`, `alias`, and `require` are all lexically scoped. First off, scope is just the _space_ in a program that something is accessible.
So a module provides a space where fuctions are defined, and that space is referred to as the Module's scope. Modules are essentially big cages where
cute little functions animals hang out. If something is dropped in the cage (aka imported) well now all the cute little functions can interact with
that new thing and everything in it. But let's say we only want one function to have something (the other's might get a ittle pudgy if they also got it).
 Well that's cool because each function is actually super territorial and has it's own space where only it can interact with things.

So yeah, lexical scope...

Anywhoooo you don't want your functions getting entitled and fat so only import the stuff that you really need.


### Alias

Alias creates an alias for a module.


```elixir

defmodule MyModule do
  def do_something do
    IO.puts "What do you want me to do?"
  end
end

def AnotherModule do
  alias MyModule, as: Mod
  def do_something_again do
    Mod.do_something
  end
end

```

It's not really helpful in this case because the Modules are pretty tiny, as are their names. But one thing I forgot to mention is that you can
nest modules, and in that case you might end up with something like `BigAssModule.SlightlySmallerModule.MyModule.do_something` any time you want
to write your code. So it might make sense to use an alias to make that a smidge smaller, and not gunk up your code. It also is helpful to know
that `alias` with out `:as` will default to whatever is lowest nested module in our case it'd be `MyModule`.


### Require

Require is specific to macros. macros are essentially functions that manipulate code as opposed to data. If you put a macro definition in one module,
and then use it in another, you must `require` the module in order to use the macro. Basically, the `require` directive is just a function that tells
Elixir, "Hey, before you try and execute that macro, why don't you do me a favor and load up it's definition first. Thaaaaankss!". That way all the macros
defined in the must `require` the module in order to use the macro although you could also use `import`, but don't actually it's kind of excessive.


Sweet so that's pretty much it. There's also the `use` macro you might want to checkout. Overall, when it comes to these directives just do the smallest thing
possible. i.e. if only one function needs something from another module only use the directives in that function, and if you happen to be importing, just import
what you need. Finally, when using macros it's convention to use `require`.
