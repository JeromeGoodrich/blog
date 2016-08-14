---
layout: post
title: The Repository Pattern
date: 2016-05-20
---

Ruby on Rails includes the massive Object Relational Mapping(ORM),
Active Record. The use of an ORM API allows you to write code to access
an underlying database using the natural features of a given language.
It's the difference between

THIS

```java
    String sql = "SELECT ... FROM persons WHERE id = 10";
    DbCommand cmd = new DbCommand(connection, sql);
    Result res = cmd.Execute();
    String name = res[0]["FIRST_NAME"];
```


AND THIS.

```java
    Person p = repository.GetPerson(10);
    String name = p.getFirstName();
```

In the case of Rails and Active Record this effect is amplified. Because
Rails contains so many different and helpful methods and conventions,
the use of Rails and Active Record together becomes such a seamless
experience that the framework and underlying database appear inexorably
linked. Despite this, it's important to realize that Rails and the
database it is using are simply implementation details. They are modular
pieces of a a software puzzle that ought to just as easily include Rails
and Postgres as Django and SQLite depending on your needs. However, the
tight coupling of Rails and it's database through Active Record make
this modularity rather difficult without some thoughtful changes to
design of a typical project. Enter the repository pattern...

The repository pattern can be thought of as an interface between the
database and whatever is using it. In a typical Rails project the
controller would interact directly with an Active Record object, and
thus with the database. The repository pattern changes this interaction
by wrapping the active record object. Now the only thing the controller
knows about is a repository, which could contain an active record,
another ORM, or in memory implementation. The point is the controller no
longer has any idea about the database being used and thus any kind of
database/ORM can be used.

The repository pattern takes care of decoupling the database from the
main app, and a similar approach called the presenter pattern is used to
do the same with the UI.

for a more in depth of the [repository
pattern](http://blog.8thlight.com/mike-ebert/2013/03/23/the-repository-pattern.html)
