# Architectural Styles and the Design of Network-based Software Architectures

These are the notes I made reading
[Roy Fielding dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm),
from where REST originated.

Why read it ? Well, everything these days is RESTful, which is a good hint that
probably almost nothing is really RESTful =P. Almost everything I read
about REST is really poor on explaining true advantages and architectural
advantages. Usually people only talk about URIs and CRUD, which is nice
but does not seem as such a big advantage. There is value on having consistent
methods and behaviour for CRUD operations, but almost every service that
is truly interesting is much more than a simple CRUD, CRUD only dictates
how to handle the lifecycle of resources on a uniform way, but it says nothing
on how to model resources or how to relate/compose resources to build
complex systems.

So I read the dissertation as a part of an exercise on finding this =).

## Dedication

It is interesting to write about the dedication, but it was too awesome
to ignore. Since we have a lot of similarities with actual architecture
the quote made on the dedication is extremely well placed:

```
Almost everybody feels at peace with nature: listening to the
ocean waves against the shore, by a still lake, in a field of grass,
on a windblown heath.

One day, when we have learned the timeless way again,
we shall feel the same about our towns, and we shall feel as much at
peace in them, as we do today walking by the ocean, or stretched out in
the long grass of a meadow.

-- Christopher Alexander, The Timeless Way of Building (1979)
```

The Timeless Way of Building has already been mentioned by Kent Beck
as an awesome source of inspiration on what software architecture should
be inspired, the problem that you are solving, the people that are
going to use that architecture everyday.

Software has two audiences, other developers that have to evolve the
system and clients that just use the system. The definition of a successful
architecture is when both are extremely pleased, and being pleased comes
through simplicity, just like nature, it simply fits and feels good.

It is a social phenomenon, deeply influenced by human cognition.
The dissertation has a **VERY** good start =).

## Abstract

A definition of architecture from the abstract:

```
Software architecture research investigates methods for determining
how best to partition a system, how components identify and communicate
with each other, how information is communicated, how elements of a
system can evolve independently, and how all of the above can be described
using formal and informal notations. 
```

Makes perfectly sense to me, specially because it has nothing to
do with technology. Not a single one of these things make necessary
to decide which broker or database you are going to use.

How information is communicated will usually mean defining a serialization
format, but as we will see in details further, this aspect on REST is
easy to evolve and it is decoupled from the resources, serialization formats
are just representations of the actual resource, you can have multiple
different representation of the same resource. So how to model resources
and partition the system is still predominant, instead of these details.

Not that details are not important, but they should not influence
architecture, the input for architecture are the developers and the
clients.

On the focus of REST:

```
REST emphasizes scalability of component interactions, generality of
interfaces, independent deployment of components, and intermediary components
to reduce interaction latency, enforce security, and encapsulate legacy systems.
```

Looks good to me, specially to architect systems that involves multiple teams
and evolve along time. Independent deployment of components is awesome, I 
was able to see with my own eyes systems that where developed as 
"RESTful microservices" but had to do updates in lockstep, full releases
involving multiple services had to be tagged and updated at the same time or
stuff would break. By definition they where not REST. To be honest it seems to
me that a service can't be RESTful by definition, REST is an architecture so
it involves all the services and how they interact, inclusive how deployment
can be made.

## Introduction

The introduction is also awesome, quoting:

```
A good architecture is not created in a vacuum. All design decisions at the
architectural level should be made within the context of the functional,
behavioral, and social requirements of the system being designed, which is a 
principle that applies equally to both software architecture and the traditional
field of building architecture.

The guideline that "form follows function" comes from hundreds of years of
experience with failed building projects, but is often ignored by software
practitioners.
```

The first thing that pops out to me is **social requirements** because this
is ignored by developers all the time. Im not sure if it is because we
are "technical" people, but architecture is about people actually, not
technical stuff that will make you feel awesome about yourself.

Another awesome quote is **form follows function**. This conveys the idea
that form must exist, you can have a sense of wonder and art with your code,
with its form, but it **MUST** follow function, not the other way around.

This is has special meaning to me because I have developed several
architectures that where entirely to please myself, the function was following
the form that I liked more. When the subject is technology this is even worse,
the desired technology (usually stuff that is trending) comes first, again
function follows later.

More about that, quoting from the dissertation:

```
The funny bit within the Monty Python sketch, cited above, is the absurd
notion that an architect, when faced with the goal of designing an urban
block of flats (apartments), would present a building design with all the
components of a modern slaughterhouse.

It might very well be the best slaughterhouse design ever conceived,
but that would be of little comfort to the prospective tenants as they
are whisked along hallways containing rotating knives.
```

The sketch is really funny, I myself developed several "slaughterhouses",
just because I was in love with "slaughterhouses". I was creating on
the vacuum, following only my sense of aesthetics, good architecture is
is not created in the vacuum.

A final quote:

```
The hyperbole of The Architects Sketch may seem ridiculous,
but consider how often we see software projects begin with adoption of
the latest fad in architectural design, and only later discover whether or
not the system requirements call for such an architecture.

Design-by-buzzword is a common occurrence. At least some of this behavior
within the software industry is due to a lack of understanding of why a
given set of architectural constraints is useful. 
```

The irony is how REST is being "adopted" ubiquitously without any understanding
on why the set of architectural constraints is useful (I'm pretty sure I did
and I see other people doing it).

Really good ideas take time to understand and apply, this does not match
well with the "fast" pace of technology fad (emphasis on quoted fast, because
it is not really fast, it just needs to sell a lot).

A little more on the objectives of REST:

```
Suppliers of information services must be able to cope with the
demands of anarchic scalability and the independent deployment
of software components.

An architecture for the Web must therefore be designed with the context of
communicating large-grain data objects across high-latency networks
and multiple trust boundaries.
```

So, if your system don't need to:

* Anarchic scalability
* Independent deployment
* Large grain data transfered across high-latency networks and multiple trust boundaries

Perhaps you should consider something else that is not REST.
