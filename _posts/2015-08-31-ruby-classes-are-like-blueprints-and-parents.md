---
layout: post
title: Ruby Classes are Like Blueprints and Parents
date: 2015-08-31
---

Before we get into this it's important to know the fundamentals. First,
**All objects in Ruby are instances of a Class.** Second, **all Classes
are objects, and the only objects capable of "creating*" *other
objects.** Classes create new objects that are endowed with the same
behaviors and attributes that are defined within the class. This was
kind of tough for me to wrap my head around, at first. However, as with
most things, analogies and putting things into plain english helps.
Classes are like general blueprints for stuff you want to build. When
you create the class you get to decide all the behaviors and attributes
that you want the things built off of that blueprint to have.  For
example, let's say you create the Building class. In this blueprint you
want to include all the behaviors and attributes you want your buildings
to have. You could put things like height, material, location, etc. so
that whenever you build something from your blueprint, (**we call this
instantiating the class**) it will automatically have those properties.
the cool things is, that you could have a skyscraper instance of your
Building class, and an igloo instance as well. They both share the fact
that they have a height, are made from materials, and have a location.
They are built from the same blueprint but are radically different.
These instances of the Building class could also have other properties
that no other instances have. A skyscraper could sway, for example.


Thinking about a class as a blueprint for instances of that class helped
me internalize the concept of instantiation, but there's one other
crucial concept involving ruby classes that lends itself to another
analogy.

Objects get their behaviors from several different places. they can get
them from their class. Remember, **all objects are instances of a
class.** They can get them from singleton methods that you define for
the object, like the object skyscraper being able to sway, and they get
behaviors from their superclass, super-super class, etc. (there are more
ways for objects to get their behaviors, but for our purposes we'll stop
here).

When an object get's it's methods from a super-class or it's ancestors,
we call that inheritance. **Inheritance is class specific, meaning it
only applies when we are dealing with classes**. Inheritance lends
itself nicely to an analogy about family and generations. We can think
of a superclass as a parent of it's subclass. When the subclass is
created, it inherits all the behaviors of the superclass, kind of like
how we inherit our genetic materials from our parents. There's one big
caveat though, while we have two parents, Ruby classes can only have
one, this is the principle of single inheritance.

It can be pretty easy to get instantiation and inheritance mixed up. I
use the analogies of a blueprint and parents respectively to keep things
straight. I think the biggest thing to remember is this: Inheritance is
Class -&gt; Class relationship and instantiation is Class -&gt; Object
relationship. The only exception to this is when Classes are actually
created.

Ruby comes built-in with a class called Class (so meta!) it has a whole
host of methods defined within it. When you create a new class in Ruby
you are actually creating an instance of the class Class. **Remember,
all objects in ruby, including classes, are instances of some class**.
Other than that, the difference between instantiation as Class -&gt;
object and Inheritance as Class -&gt; Class holds.

There's so much more to discuss regarding instances and classes like,
getter and setter methods, instance variables and attributes, class
methods and constants. but I think this is where I'll stop for today.
