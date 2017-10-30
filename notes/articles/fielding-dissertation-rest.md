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

## Software Architecture

The first chapter is about defining exactly what is software architecture.
It is very detailed, so here I will give an overview of what has
seemed like the most important parts (at least for me).

The concepts defined by an architecture are:

* Run-time abstraction
* Elements
* Configurations
* Properties
* Styles

The runtime abstraction is the idea that an architecture should hide
runtime details about how components runs, no detail about languages
or VMs should be included at this level. They are important details,
but not on this level. Architectural views are like fractals, to get
to runtime details you would be on a level that include code organization
and other details that are irrelevant when you are discussing distributed
systems.

The elements are the base of representing the architectural view,
basically:

* Component
* Connector
* Data

Components would be the services, or anything that may transform data
and state.

Connectors are how data is transmitted, or how the components
integrate. The difference is that Connectors should never transform data,
only transport it.

Data is pretty self explanatory, how information is represented inside
the system.

The dissertation is about networked systems, but I'm stupid and everything
is easier for me to understand with unix pipes, an example:

```
ps -ef | grep hi
```

The components are ps and grep.

The connector is the pipe mechanism provided by the operational
system (no transformation on the data).

The data is the constraint established by the unix architecture,
a text stream.

Now we have configuration, which has nothing to do with usual
software configuration, it means different ways to compose the
available compoments/connectors/data.

For example, this is a configuration:

```
ps -ef | grep hi
```

This is a different configuration of the same architecture:

```
netstat -anp | grep hi
```

The properties of an architecture are achieved by the constraints
that the architecture define. For example, the unix architecture clearly
sets constraints to enforce a uniform interface between components,
the text stream.

The properties obtained from these constraints are:

* Reuse of components
* Configurability (with configuration as defined before)

An architectural style will be a set of constraints. The word
**style** usually makes people think about taste, but this
should be the opposite with architecture. Quoting the text directly:

```
Some architectural styles are often portrayed as "silver bullet" solutions for
all forms of software. However, a good designer should select a style that
matches the needs of the particular problem being solved [119].

Choosing the right architectural style for a network-based application requires
an understanding of the problem domain [67] and thereby the communication
needs of the application, an awareness of the variety of architectural styles
and the particular concerns they address, and the ability to anticipate the
sensitivity of each interaction style to the characteristics of
network-based communication
```

So styles is not the same as taste, and choosing architectures using
fashion is the opposite of what an architect should do. Software
developers should strive to know as much architectural styles possible,
the constraints of each ones, the tradeoffs and in which contexts
they will perform better.

But besides all, you should understand the problem domain WELL.
Another interesting thing that I noticed when the methodology for
creating the architectural style is explained:

```
Finally, the updated Web architecture, as defined by the revised protocol
standards that have been written according to the guidelines of the new
architectural style, is deployed through participation in the development
of the infrastructure and middleware software that make up the majority of
Web applications.

This included my direct participation in software development for the Apache
HTTP server project and the libwww-perl client library, as well as indirect
participation in other projects by advising the developers of the W3C libwww
and jigsaw projects, the Netscape Navigator, Lynx, and MSIE browsers,
and dozens of other implementations, as part of the IETF discourse.
```

Why is it so interesting ? Because it shows the correlation between
defining a system architecture and development/deployment of the system.
I'm not even sure of the benefits of considering these as separated
activities (architecting/designing and implementing/deploying), but even
if you consider them separately it is important to be aware of the feedback
that development and deployment can give to you about your architecture.

The sequence described is actually a repeating cycle, reinforcing the
feedback feeding the improvements on the architecture:

```
Although I have described this approach as a single sequence, it is actually
applied in a non-sequential, iterative fashion.

That is, over the past six years I have been constructing models,
adding constraints to the architectural style, and testing their affect on the
Web's protocol standards via experimental extensions to client and
server software
```

The methodology is explained in great detail in a formal way, but the final
work of creating REST seems to have been a pretty hands on experience,
involving a lot of coding and experimentation.

## REST

I'm going to coalesce the rest of the dissertation as **REST**,
which begins at chapter 5.

Since REST has to solve the problem of integrating systems that
crosses organizational boundaries it is fundamental to provide a
uniform interface, enforcing generality on this interface.

This makes integration a lot easier, but has the setback of all
generic things, it is not the **best** implementation for no
specific problem. So depending on the specific needs of your application
the REST uniform interface may be a huge pain to implement since it is
not the best suited for your problem (it was design for high decoupling
of services and moving coarse grained data through fucked up networks).

It is important to be aware of that before dwelling further on REST
definition, it is not a panacea or a silver bullet, it is the best
thing we could come up until now to integrate the whole world, which
is usually not the case on a lot of distributed systems that do not
cross more than one organization (sometimes do not cross even one team):

```
The trade-off, though, is that a uniform interface degrades efficiency,
since information is transferred in a standardized form rather than one
which is specific to an application's needs.

The REST interface is designed to be efficient for large-grain hypermedia
data transfer, optimizing for the common case of the Web, but resulting in
an interface that is not optimal for other forms of architectural interaction.
```

There are four main constraints that promotes uniform interfaces on REST:

* Identification of resources (URL)
* Manipulation of resources through representations (Decoupling, content negotiation)
* Self-descriptive messages (Stateless)
* Hypermedia as the engine of application state (The "state transfer" on REST =P)

The core concept of REST revolves around the resource.
There is a clear distinction on REST between a resource and
a resource representation. They will even have different metadata.

For example, a resource metadata is its URL, its is a location to
access the resource representation. The resource may have multiple resource
representations, some can be added and removed along time, but the URL
remains the same since it refers to the same resource (hence the difference
between the resource and its representations, it is a 1 -> N relationship).

The resource representation metadata can be its media type,
cacheability, etc. It is imperative that manipulation of resources always
happens through representations of the resource, never the resource itself,
hence promoting decoupling since the internal resource can be modified and
evolved freely if the representations remains the same (new ones can be added
without interfering on old ones, this reminds me of versioning of content type,
instead of URLs, which may make sense depending on the semantics of the change
you are making).

### The Cookie Monster

I never thought about cookies matching the idea of stateless
on REST, and on the dissertation I got confirmation that they
are not. Actually they are one of the most notable ways people
disregard REST constraints, and on this case at the cost
of people's privacy and protocol visibility.

When cookies are present usually this allow tracing even across
sites, which violates users privacy. Beyond that you loose
the visibility property that is induced by self describing
messages. The protocol is not explicit anymore. Requests
may have different responses just because of some opaque
ID (the cookie).

When debugging interactions between a client and a server
with cookies just inspecting an isolated request is not
enough, the whole set of requests must be analyzed. Cookies
enable a cross resources / global state manipulation
that can get pretty hard to debug.

It is interesting how people are willing to kill because of
URL naming and HTTP methods but seem to have no problem at
all with using cookies to do all sort of crap.

### RPC

I always had a hard time differentiating RPC from REST.
There is some sense on thinking that REST is more resource
oriented etc, but this is a little fuzzy to me.

The author actually uses something that I never thought before
that is associated with the need for caching.

On REST, the nodes between the endpoints (client <-> server)
are not just routers, they can be caches, proxies, etc. This is
not the case on RPC, RPC always involves the communication
between the two endpoints and the nodes on the network are
just routers/brokers (all the ones that I know matches this
description).

It may not be the only difference, but it is the one that is
more clear.

## Java VS JavaScript

There is a very interesting part where an analysis is realized
on why JavaScript won the browser war against Java.

It is not related to language quality since Java was widely
adopted on other niches, and was also not a matter of marketing
since Sun Microsystems was very good at it.

In the end JavaScript understood the REST/Web principles
better, specially on these points:

* Focus on low latency
* Visibility
* Low learning curve

Javascript was just as visible as HTML, everyone could
inspect and debug it just as they used to do with HTML and
other plain text formats. Basically the choice of
using the source code and interpreting it on the
browser was a big win against having to download some
black box binary to run on a VM.

The deployment of Java code was not just opaque, it would
also lack progressive rendering. To start to show any kind
of progress it needed to download all the binaries, which
greatly increased latency.

JavaScript was also faster to starting to hack something
small and grow from there (although it is arguable if
the medium to large software can be properly designed with
it on the wild).

# Conclusion

The dissertation does a **GREAT** job on building a framework
to think about software architecture for network based systems.
It teaches a good way to think about how architectural constraints
can induce architectural properties, at least it was very
enlightening to me.

It also does a great job to explain the desired properties on REST,
which will not be the same of most organizations. Not everyone needs
to handle anarchic scalability, high levels of decoupling and the
transfer of coarse grained data.

On the web systems that where never intended to work together
need to be composed, that is why there is a great deal of
constraints trying to enforce uniformity. As the author says,
this will produce a very generic protocol that will be great
as a way to integrate the whole world but will perform poorly
on more specific tasks.

Always think about what are the architectural properties that
you need to solve your problem and them think about constraints
that will satisfy them, don't just do RESTful for the sake of
being RESTful.
