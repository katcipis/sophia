# Software Architecture

Some material/ideas I found along the way on software architecture.
Also some personal rants.

## REST Dissertation

Roy Fielding's [Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
is a gold mine of advice on how to deal with architecture of distributed systems.
Not in a stupid "use REST for everything", but in a very thoughtful way.

I have some notes [here](./articles/fielding-dissertation-rest.md).

## Good Architecture presentation: Principles

Bases:

* https://vimeo.com/43612849

* Makes intention of the system clear (not tools)
* Allows to defer tools decisions
* Focus on the core logic of the problem you are solving (not related to tools)
* Decouples logic from UI
* Decouples logic from delivery mechanisms (HTTP/RPC/WHATEVER)
* Decouples logic from persistence (the database, of course)
* Decouples logic from frameworks of any kind

About intention, since the metaphor used is about architecture, the plant of a building
usually allow you to infer what kind of a building it is (school, library, church).

Architectural diagrams and project directory layout should do the same. Im not interested
on the framework chosen, or if the system is a web system using Rails. First I want to
know the intention of the system as a whole, what it does, them I can search for how.


## Async all The Things

Currently there is a movement on building distributed systems always as an 
asynchronous system, basically using pub/sub or queues for all integration.
People literally say that anything request/response based, sync, is just
worse because it scales less and is less resilient etc etc. Which is not
true TBH, there is always ways to improve resiliency in both models, and also
ways to lose it, and async is remarkably good at losing resiliency in ways
that is not obvious (you don't see any errors, but nothing works either).

One interesting read on this is [here](https://cpitman.github.io/microservices/2018/03/25/microservice-antipattern-queue-explosion.html#.Y2KFmNLMIUE).
The rant here is against this idea of integrating everything with pub/subs and
queues. Not against using them where it makes sense.

I advocate that async vs sync is defined by the domain you are modeling,
the problem you are solving, it is not about tech or what engineers prefer.
It is also not about resiliency, because request/response can be quite resilient
(the web is basically request/response/sync for a lot of things).

But lets focus on the ways that async/queues makes things more complex, because
they do, they may provide benefits but the costs must be clear, since people
usually focus on benefits I will focus on the costs :-).

To start, by definition it is harder to code around an async API and you have
more failing points, instead of service A + service B you now have
service A + broker + service B, that is by definition more moving parts
and more things that can go wrong.

People usually focus on the benefits the broker brings, in terms of features,
but forgot that they will be an extra moving part nonetheless that can
affect the whole behavior of your system.

One of the usual benefits is around resiliency because some brokers implement
sophisticated try again/backoff logic to help handle situations where something
is wrong on the system of with a message, with also support to some form of
dead letter mechanism. There is nothing wrong with any of that, but it still
is a more complex way to reason about resiliency. Now instead of considering
the behavior of 2 endpoints you need to consider the behavior of 3 endpoints
in terms of failure recovery.

Also smart message brokers are a double edged sword. If you are old enough
(I am) you can remember that when the whole RESTful thing started more than
a decade ago, one of the points where exactly that middlewares and message
brokers that were smart were bad and we need to go back to a place where you
have dumb pipes and smart endpoints, there were even some analogies with
shell pipes, etc. So in a sense it feels like the industry is just forgetting
(as usual) lessons from the past and going again in the same direction.

Why the smart brokers didn't work out well ? I believe it is the lack of cohesion
for very core behavior, not just fault tolerance but even specific knowledge
ends up in parts of the system that you don't expect them. For example, in a
system with a service mesh + a really smart message broker/queues/etc, it is really hard
to understand where try agains/backoff logic will happen, it will probably
happen at both in ways that may not be compatible/desireable. You also have information
that is domain specific that affects fault tolerance implementation, like
which operations are safe to retry, for how long, etc. It ends up being
harder to have a cohesive view of these invariants, because instead of
the services being responsible for this it is delegated to the communication
channel. So part of your domain is on the service responsible for solving
that problem and part of it is defined on some YAML configuration of a
broker/service mesh/etc. Anyway I digress, this is not even about service meshes.

Going back to the async/queue model, the code itself is also harder,
on the client side you need to send something on a queue and now you have to
find a way to wait for an answer because in a lot of scenarios it
involves waiting for an answer with queues (it rarely is fire and forget)
and now you need some polling.

While with request/response it is literally one line, you send a request and
get answer right away. You can't beat request/response/RPC because nothing
can be simpler than a single function call.

You would need to create a function that does something similar but does the
polling waiting for the answer. Which is usually something that is not standard,
so you need to reimplement this polling over and over again
and you need some way to correlate the answer, so now you need a job/req ID,
manually handled and if that collides you are fucked.

I know for a fact that this can go wrong, because the start of my experience with
the whole lets use queues and async things for everything stravaganza was with code
that was being clever, using a Redis Task Queue for doing something that was
obviously synchronous. Why it was obviously synchronous ? The operation was
purely functional (no state) and if it wasn't computed in 10 seconds or less
it was useless to get a response, so in my opinion it would be an embarrassingly
synchronous operation. The Redis Task logic was encapsulated in a function, so
the code looked simple, like a request and response, but it wasn't. And the nice
thins about abstractions is that you need to open them up when they don't work.

The solution was the usual on the case of nasty bugs, re-design, because most
bugs are caused by bad design. The new design ? The simplest REST request ever
written on the history of mankind. And it worked for years while I worked there,
and AFAIK it is used until today. And that is where my whole intuition with
this topic started to build, always going for some task queue is just a bad
idea, with emphasis on **always**.

Flow control is also more complex. In request/response you can have a very
standard/simple backoff protocol, like in HTTP using a 503.
With queues there is never a standard way. In my experience people just don't
do flow control and end up with a huge queue which then can cause problems.

It is specially a problem if the problem you are solving is time sensitive,
like the client is waiting and will not wait forever. They will wait for like
a minute lets say and then give up. In a situation like that an ever growing
queue will just jeopardize your entire system, and it will do that in a way
that is against the idea of failing fast.

With request/response you can have timeouts and backoff protocols that allow you
to fail fast, with a queue you will just wait forever for an answer and never
get it. You can do a polling with a timeout, but it is more complex than a
simple RPC with a timeout.

Also after the timeout happens and you give up, now you have the problem of
answers to requests on the queue that are orphans, because the user already
gave up on them, which is the problem of cancellation. What to do with them ?
Something needs to drain them. All of those are no problems on request/response,
if you give up on a request you cancel the connection/stream and the other
side will log an error and that is it, as simple as possible.

What I mean is that for a scenario that request/response is a good idea,
like low latency quick requests, it is fairly clear that enforcing async on
top of it will only make things more complex, even if it is with the good
intention of failure tolerance will be better/easier (although I would
argue that that is not true depending on the failure model).

But of course there are more subtle scenarios, that lend themselves more easily
to async, then the discussion can be more rich.
For example, in a banking system, transfer for sure are async,
because the real world problem they solve literally can take days.
So something async is very suitable here.

In the end it is a mindset. You are going to be wrong anyway, I just prefer to
be wrong on the side of "it is way too simple and is not enough".
Specially because being way too simple is easier to detect.

When things are way too complicated but at least seems to be enough it is
trickier to see you have a problem. Because you need to detect that even
though it works, you only need a subset of the solution.

This reminds me of something said by Hoare:

```
I conclude that there are two ways of constructing a software design:
One way is to make it so simple that there are obviously no deficiencies
and the other way is to make it so complicated that there are no obvious deficiencies.
```

This makes it clear why it is hard to argue around complex async systems,
complex systems have no obvious deficiencies because they are so complex that
finding these deficiencies is not trivial.

But that does not mean the deficiencies are not there.
So complex systems are comfortable in a scary way.


## Procedures inside the database ?

* Putting procedures inside the database is the utmost level of coupling with the database
* It is a tradeoff from latency to strong coupling, you are coupled to the technology now
* If you need to change the database for some reason, you are going to have a bad time (eg. Neoway)
