# Documenting Services

Some notes on how to document services, what worked for me,
what didn't, etc.

## The Problem

What would be a service ? By service I mean any binary that
provides some service for you through an API that is (usually)
language agnostic. I'm trying to make a distinction between
a service and a library, libraries publishes APIs that are
accessible through the runtime of the language that they
are developed on (or through some type of FFI).

Why make a distinction ? Libraries have their code as a form of basic
documentation. Function prototypes and data structures have a lot to
say. Not that you don't need any more docs, but some documentation along
public functions is enough for me. Not that this is not an area that
needs improvement but I see that we are much better served on this area.

Golang for example have something that resembles doxygen docs and a system
to express examples that are compiled and run and embedded on the docs later.

The problem with services is that usually you are integrating with them
from language X but the service has been developed in language Y and the developer
of X probably doesn't even know which language the service he is consuming has
been written on (nor should he, knowing things is fun but having to know in which
language a service has been written at this level of abstraction seems like a
bad idea).

It may seem like the problem is to document distributed systems, which is the
first thing that comes to my mind when I think about services, but it is not.
It is not because there is a great deal to learn about how CLI tools are
documented. With UNIX pipes you can compose multiple
services together in a very elegant manner and usually there is no coupling
on which language each tool has been written, the only thing that matters
is the protocol (streams in the UNIX case), which is pretty much the case
on distributed systems.

## To document or to not document ?

Ok this seems like a stupid question, but it is important to analyze with
honesty. The need to document your APIs are to help clients of your
service to understand how to use it. These clients will even be you after
some months.

Although almost no developer that I know likes to document,
my reasons for not liking it:

* No short term benefit (perhaps medium/long term)
* Documentation is basically duplication
* It is duplication that you can't run, it is boring

To mitigate some of these I try to document before (like tests with TDD)
so the act of document helps me to think about the specification of
the service, so there is a better short term benefit. Letting to document
on the end is ask to not document anything at all, there is very little
incentive (humans are pretty bad at thinking medium/long term, specially
if the task is annoying).

Having this is mind I always try to document as soon as possible
(usually before) and I document as little as possible, where possible means
other people not involved on development can understand what the service
does and why, and how to integrate with it.

The more details you document, more you are duplicating from the code and
the chances that your docs will be outdated. So your service docs should focus
on how I can use the service, not internal design decisions of your service or
which language it is written in (if your protocol is not coupled with those
neither should the docs).

## How to document ?

Here I will elaborate on how I used to write docs for services,
how I do today and how I want to do it tomorrow. The tomorrow
is just a vision/direction, I still have no idea on how
to implement some details.

The challenges of good documentation that I use to compare
different techniques are:

* How easy is to keep it updated with changes on the service ?
* If you have discovered the service, how easy is to discover the docs ?
* How easily can you match a running version of the service with the docs ?

There is the obvious "can I understand anything that you are saying ?", but
this is more related to the contents of the docs and currently I don't approach
it with any formalism, so you just need to explain well and provide examples =).

Also these 3 questions are decoupled from the contents of the documentation.
The contents are essential but they are not related to any of those challenges.
Since they seem to be orthogonal I'm going to focus on those 3.

### Past

In the past I used to document services APIs on an wiki pages. It was
a on premisses wiki service on the place I worked at the time.

It is not hard to think on why this was a bad idea, using the test

#### How easy is to keep it updated with changes on the service ?

Not very, sometimes people adding code and functionality to services
did not even know that the docs existed, they where pretty far from
where the code resided and the tooling to edit them was pretty
different too (people coded with text editors, the wiki was edited
on the browser).

#### If you have discovered the service, how easy is to discover the docs ?

As hard as it can get. With luck there would be a link on the project
pointing to the wiki link. But now the problem of the service client
is to find the project. From a system deployed in production finding
the project where the source code is stored is not easy. This is
usually mitigated with emails or just asking someone on the coffee.

#### How easily can you match a running version of the service with the docs ?

Good luck with that. With proper tagging on the repository and discipline
on how things are documented (like a "since v2.0" on the docs) you may get
some matching done, but in a model where the docs are so far away from the
code that people do not even know it exists you are grasping at straws.

#### Conclusion

Are wiki pages bad ? Hell no. Are wiki pages a good idea to document
services API ? Hell yeah. I would use wiki pages only to document knowledge
that is decoupled from any individual service, like research material,
even brainstorming (the [c2 wiki](http://wiki.c2.com/) for example)
but never for documenting services API.

### Present

### Future
