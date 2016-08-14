---
layout: post
title: Metalinguistic Abstraction
date: 2016-04-14
---

> Expert programmers control the complexity of their designs with the
> same general techniques used by designers of all complex systems. They
> combine primitive elements to form compound objects, they abstract
> compound objects to form higher-level building blocks, and they
> preserve modularity by adopting appropriate large-scale views of
> system structure. In illustrating these techniques, we have used Lisp
> as a language for describing processes and for constructing
> computational data objects and processes to model complex phenomena in
> the real world. However, as we confront increasingly complex problems,
> we will find that Lisp, or indeed any fixed programming language, is
> not sufficient for our needs.We must constantly turn to new languages
> in order to express our ideas more effectively...
>
> *...Metalinguistic abstraction*--establishing new languages--plays an
> important role in all branches of engineering design. It is
> particularly important to computer programming, because in programming
> not only can we formulate new languages but we can also implement
> these languages by constructing evaluators. An *evaluator* (or
> *interpreter*) for a programming language is a procedure that, when
> applied to an expression of the language, performs the actions
> required to evaluate that expression.
>
> **The evaluator/interpreter, which determines the meaning of
> expressions in a programming language, is just another program.**
>
> -Structure and Interpretation of Computer Programs

The most interesting and impactful sentence in the 400+ pages of SICP
I've read so far is the bolded sentence above. The also say as much in
the book, calling it "the most fundamental idea in programming". I'm
still gnawing on it, and while I think the importance is definitely
evident, I still struggle to explicitly state why.

The book walks through building an Lip program that evaluates Lisp
expressions aka an interpreter, and uses the analogy that a program is
simply the description of an abstract machine. An interpreter, thus is a
special type of machine that takes the description of other machines as
input, and then configures itself to emulate the machine described. This
is where I think SICP started to impress upon me just how important that
sentence is.

If you give a Lisp interpreter that evaluates Lisp expressions a Lisp
program that behaves as an interpreter for some other language like C.
The Lisp interpreter will emulate the C interpreter, and now can emulate
any machine described in Lisp. In other words, any interpreter can
emulate any other, meaning that what can be computed is independent of
the language and computer.

There's so much more to this including optimizations, different types of
interpreters(normal(lazy) vs. applicative order), the difference between
interpreters and compilers, but I think it's a fundamental topic that it
helps to have some good foundational knowledge of. So to that end, I
found [this site](http://www.sicpdistilled.com/section/4.1/)  which
provides a distillation of some of the SICP concepts but more
importantly walks through how to actually build an interpreter in
Clojure. hopefully I can find some time to do that.
