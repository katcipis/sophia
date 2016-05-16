# The Go learning experience

We started to work at Neoway with Golang for almost a year right now, and on this post
I will try to pass on some of the experience of learning Go.

To make things easier I will use a very clear example, one service, and some context
on what was happening.

By no means this is a representation of the experience of everyone at Neoway, there is a lot
of teams working with Go. This is a very narrow and personal view on how was to me the process
of learning and developing Go code.


## Context: The Datapirates Team


Our team is responsible for capturing data on the web. So we basically develop
scrapers and all services required for the scrapers to work properly
(like captcha breaking, proxy providing, etc).


## Context: Current Architecture

Here at Neoway we work with business intelligence and fraud prevention. We do so by:

* Gathering a great deal of data from a lot of different sources
* Cleaning it
* Normalizing it
* Aggregate and correlate with the rest of our data

So, it is clear that we have a data pipeline that feeds our application with data.
Also we work with predictive models, another client for the data.

This data pipeline currently consists on a series of postgres databases with all the
logic implemented on PL/SQL and some data pumps copying data from one database to another.

It worked great for a lot of time, and it is still paying our salaries, but it wont scale
enough to take us where we want to go.

The lack of scalability is both technical (scaling postgres) and on engineering effort.
By engineering effort I mean how we maintain what we have, how to add new stuff, how to test it.
With everything coupled to the databases, none of this where very easy.

Enters the new architecture


## Context: New Architecture


The new architecture starts with very clear goals that we must achieve:

* Clear service boundaries, with clear responsibilities for each service
* Clear boundaries means good automated tests
* Good automated tests means easy to build development environments
* Use the simplest tools as possible to get the job done
* Make the intention of the system as clear as possible with the architecture
* The most important, **correct data** data has to go pretty fast to the user

The idea here is not to get in great detail on the architecture, this post is about
Go after all. But this is part of why we used Go, so it seemed to me important to create
this context.


## Journey begins

On the new architecture we had some new services that where good candidates to be
developed in Go. So we decided to give Go a try.

Everyone has different reasons to think that Go is a good choice. For me the main ones
are simplicity and familiarity.

//TODO simplicity, mention rob pike article
//familiarity, why not LISP ? :-)


## Dependency management


## Coverage


## Parallelism problems

TODO: Talk about the problem with each package executing in parallel (-p) thing
