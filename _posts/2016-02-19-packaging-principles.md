---
layout: post
title: Packaging Principles Pt. 1
date: 2016-02-19
---

Today I want to talk about Packaging Principles as they are discussed in
PPP and Uncle Bob's column in *C++ Report*. Building off of the SOLID
principles and how they apply at the class level of code we must next
acknowledge that:

> As software applications grow in size and complexity they require some
> kind of high level organization. The class, while a very convenient
> unit for organizing small applications, is too finely grained to be
> used as an organizational unit for large applications.
> Something “larger” than a class is needed to help organize large
> applications.
>
> *-Robert C. Martin, *C++ Report*, Nov-Dec 1996*

This larger granule in the context of PPP is called a package. Java jar
files, Ruby gems, and javascript libraries can all be thought of as
examples of packages. As I understand them, there are two important
things to remember about packages:

1.  Packages can be thought of a group of classes that are grouped
    according to some criteria.
2.  A large application will likely have many different packages and
    those packages will possibly relate to each other in some way.

The first 3 packaging principles relate to the concept of package
cohesion and deal mostly with identifying what sort of criteria it makes
sense to use in order to group packages into packages. While the second
3 packaging principles concern package coupling, or how it makes sense
to design an application with regards to the relationship between
packages.

**Package Cohesion Principles**

Reuse/Release Equivalency Principle (REP) - *the granule of reuse is the
granule of release*

-   Reusability comes only after a tracking system is in place and
    offers the guarantees of notification, safety and support that the
    potential reusers will need.
-   Reusability is based on packages which are comprised of classes. If
    a package is reusable **all** the classes it contains must be
    as well.
-   All classes in a package should be reusable by the same audience.

The Common Reuse Principle (CRP) - *The classes in a package are reused
together. If you reuse one of the classes in a package, you reuse them
all.*

-   Classes that have many dependencies on each other should be in the
    same package
-   It should be impossible to depend on some classes of a package and
    not others
-   Classes that are not tightly bound to each other with class
    relationships should not be in the same package.

The Common Closure Principle (CCP) - *The classes in a package should be
closed together against the same kinds of changes. A change that the
affects the package affects all the classes in that package and no other
package*

-   Single Responsibility Principle restated for packages: packages
    should only have one reason to change
-   put classes that are likely to change for the same reason in the a
    package
-   group together classes that are open to the same types of changes,
    so when a change in a requirement comes along it will be restricted
    to a minimal number of packages.

I'm going to skip the Package Coupling principles for now because I
don't quite understand them and they aren't particularly relevant at
this point in my software career. I have a general awareness of them and
I think that's enough for now.
