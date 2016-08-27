---
layout: post
title: HTTPeek - Choosing Datastores
date: 2016-08-25
---


It has recently come to my attention that I might not have chosen the best datastore for HTTPeek.
It makes sense since I put almost no thought into my datastore options and their respective benefits
and drawbacks. I think this is probably a valuable exercise so I want to try a thought experiment and
determine what might be an alternative better suited to HTTPeek. Put yourself in my shoes, how would you
make the decision?

Let's rewind a couple of months...


You've just finished working on a rails ebay clone, and are now asked to create an alternative to [requestb.in]()
You understand the following.

- Bins must be unique
- Bins may be private
- requests are made to bins and bins return custom responses
- Bins have a response they return to all requests made to the bins.
- JSON api endpoints for creating a bin, and returing all requests made to a bin
- It was going to be a short project 2-3 weeks.

That's about it. You know that the requirements are likely to change, but you aren't aware of how or when.
How do you select the best datastore for the job?


The thing that sticks out to me first, is how simple it all seems. There's a bin and some stuff associated with
that bin like a response, requests, etc. Is there a lightweight and flexible database will help us keep the data
uncomplicated? Is there even a need for more than one _entity_, and if not can we eliminate the option of cumbersome
relational databases? Yes, I'd argue, that's pretty reasonable.


Thus, relational databases like Postgres, which is what I am actually using, get the axe, whoops!
But before moving on, grant me a quick aside. It now seems rather obvious to me that the using Postgres
for this project was probably not a great choice, but I think the fact that alternatives weren't even on my
radar speaks to the pervasiveness and influenence the relational database paradigm commands.

After working on that ebay clone project that also used Postgres, it's very easy to get into
the habit of fitting things into schema's and assigning realtions to them. In fact, without much experience
working with other paradigms it's hard to envision doing it any differently.


Anyway, let's say you're wiser than me, and read the writing on the wall and determine there are better alternatives
to a relational DB. Where does that leave you? Well you'd probably do some research and come back with a top 3 or
something like that. Here are mine in no particular order: Datomic, MongoDB, Redis.


Datomic is the new kid on the block everyone's really excited about that's really innovative and different but it
also kind of knows it and hides its secrets i.e. it's proprietary. As an aspiring software crafter I'd really prefer
to work with something open source. But Datomic makes it on my list, based on sheer novelty and innovation. It also shares the same ethos
as Clojure and FP. Maybe because it was created by the same guy who created Clojure. It's very Clojure friendly. Despite how cool Datomic seems, it's not the best option for HTTPeek.


That leaves MongoDB and Redis. I don't think one of these are better than the other in the context of HTTPeek.
They're just different. From what I can gather, the main upside to Redis is it's speed. It's typically used a cache for
other DBs like MongoDB, but works well as a standalone datastore as well. MongoDB is easy to get up an running, It stores
things as JSON documents, which seems well suited for HTTPeek. Both have good, mature supporting clojure libraries, so It's
kind of a dead heat. I might lean more towards MongoDB based on the fact that the project was initially only supposed to take
a couple of weeks, but I think it would be interesting to implement both and see which I like better. Shouldn't be too hard given
the current architecture of the project.



I'm kind of surprised by how long it took me to figure out this thought exercise, and am still not entirely sure I made the right choice for the right reason, and I even have the benefit of hindsight! Overall, I think selecting a datastore for a project is a very nuanced task that probably benefits a lot from experience in working with one or another. It's really hard to say what works better than something else without actually working with either. As such, it seems what's most important is designing a flexible system that can work with whatever implementation seems best for the project. In addition, I'm also fuzzy on when it makes sense to switch datastores. I think Postgres works fine now that it's set-up but when does that change? What type of pain should I be looking for? Are there rules of thumb? Maybe that's fodder for another post.
