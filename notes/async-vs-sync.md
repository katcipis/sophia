# Async all The Things Considered Harmful

Currently there is a movement on building distributed systems always as an 
asynchronous system, basically using pub/sub or queues for all integration.
People literally say that anything request/response based, sync, is just
worse because it scales less and is less resilient etc.

Which is not true TBH, there is always ways to improve resiliency in both models, and also
ways to lose it, and async is remarkably good at losing resiliency in ways
that is not obvious (you don't see any errors, but nothing works either).

One interesting read on this is [here](https://cpitman.github.io/microservices/2018/03/25/microservice-antipattern-queue-explosion.html#.Y2KFmNLMIUE).
The rant here is against this idea of integrating everything with pub/subs and
queues. Not against using them where it makes sense. So some of the points I try to make may seem too
extreme, and they are, but the context is an argument against doing **EVERYTHING** using asynchronously.
This is not a rant against async solutions, or pubsub/streaming systems or queues/brokers.
They are remarkably useful and fundamental to solve problems that lend themselves to be handled asynchronously.

I advocate that async vs sync is defined by the domain you are modeling,
the problem you are solving, it is not about tech or what engineers prefer
or what is more resilient. It makes no sense to say that async solutions are more
reliable/resilient because there is no such thing as an async solution, you only have
async problems. If the problem is async, then an async solution for it will
be more resilient than a sync one because it is the solution that matches the problem.
When approached this way you are doing "form follows functions", which is
a great design principle. When you already start with an async solution
because "it will be more reliable" you are doing "function follows form", which
usually results in a much more complex and less reliable/resilient system (ironically).

But lets focus on the ways that async makes things more complex, because
they do, they may provide benefits but the costs must be clear, since people
usually focus on benefits I will focus on the costs from now on.

To start an async solution with a broker has more failing points.
Instead of service A + service B you now have
service A + broker + service B, that is by definition more moving parts
and more things that can go wrong.

You can also model async processing just with service A + service B, which makes the
discussion even more complex :-). I do focus on having some broker because everyone
that I discussed about this and loves async for everything usually is also in love
with the automatic retry and other features that brokers provide (like automatic providing
storage for pending events/tasks/etc). But even for a service A + service B an async
solution would require state being stored on the service modeling the async operation and
some sort of polling or some streaming of events that is protocol dependent (like SSE on HTTP).

People usually focus on the benefits the broker brings, in terms of features,
but forget that they will be an extra moving part that can affect the whole
behavior of your system.

One of the usual benefits is around resiliency because some brokers implement
sophisticated try again/backoff logic to help handle situations where something
is wrong on the system or with a message, usually with some support to a form of
dead letter mechanism. There is nothing wrong with any of that, but it still
is a more complex way to reason about resiliency. Now instead of considering
the behavior of 2 endpoints you need to consider the behavior of 3 endpoints
in terms of failure recovery (and consider what to do with the dead letter,
which usually is ignored/not handled properly).

Also smart message brokers are a double edged sword. If you are old enough
(I am, sadly) you can remember that when the whole RESTful thing started more than
a decade ago, one of the points where exactly that middlewares and message
brokers that were smart were bad and we need to go back to a place where you
have dumb pipes and smart endpoints, there were even some analogies with
shell pipes, etc. So in a sense it feels like the industry is just forgetting
(as usual) lessons from the past and going again in the same direction (the endless cycle).
But in defense of the industry (rare for me to defend it =P), the smart brokers we
have today are less smart than the past ones, they were much smarter and even contained
some domain specific knowledge of the system, the new generation of brokers are more
similar to generic queues with some retry/backoff logic, which is a much nicer set of features.

Why the smart brokers didn't work out well ? I believe it is the lack of cohesion
for very core behavior, not just fault tolerance but even specific knowledge
ends up in parts of the system that you don't expect them to be and can be hard to find.
For example, in a system with a service mesh + a really smart message broker/queues/etc, it is really hard
to understand where try agains/backoff logic will happen, it will probably
happen on both in ways that may not be compatible/desireable. You also have information
that is domain specific that affects fault tolerance implementation, like
which operations are safe to retry, for how long, etc. It ends up being
harder to have a cohesive view of these invariants, because instead of
the services being responsible for this it is delegated to the communication
channel/mesh/broker. So part of your domain is on the service responsible for solving
that problem and part of it is defined on some YAML configuration of a
broker/service mesh/etc. Of course not all meshes/brokers are born the same,
but I digress, most of the complex logic I see these days are on meshes and not message
brokers and the focus here should be on the brokers/async modeling (they are not related,
most meshes "add resiliency" on top of sync interactions).

Going back to the async/queue model, the code itself is also harder on the client side, why ?
well you need to send something on a queue and now you have to
find a way to wait for an answer which will involve waiting for an answer
on a different queue (hopefully ?) and now you need some polling.

The exception here would be if you can just fire and forget, but then you probably
have an event, and events can be pretty useful in distributed systems. That would be a perfectly
good use of a pubsub broker or a streaming system. But lets go back to simple
request/response systems being modeled as async/brokered systems because that will make
them automatically better/more resilient.

Now think about request/response, it is literally one line, you send a request and
get answer right away. You can't beat request/response/RPC because nothing
can be simpler than a single function call.

You would need to create a function that does something similar but does the
polling waiting for the answer. Which is usually something that is not standard,
so you need to reimplement this polling over and over again
and you need some way to correlate the answer, so now you need a job/req ID, manually handled.

I know for a fact that this can go wrong, because the start of my experience with
the whole "lets use queues and async things for everything" stravaganza was with code
that was being clever, using a Redis Task Queue for doing something that was
obviously synchronous/simple. Why it was obviously synchronous ? The operation was
purely functional (no state) and if it wasn't computed in 10 seconds or less
it was useless to get a response, so in my opinion it would be an embarrassingly
synchronous operation. The Redis Task logic was encapsulated in a function, so
the code looked simple, like a request and response, but it wasn't. And the nice
thing about abstractions is that you need to open them up when they break.

The solution was the usual for nasty bugs: re-design. Most
bugs are caused by bad design. The new design ? The simplest REST request ever
written in the history of mankind. And it worked for years while I worked there,
and AFAIK it is used until today. And that is where my whole intuition with
this topic started to build, always going for some task queue is just a bad
idea, with emphasis on **always**.

Flow control is also more complex. In request/response you can have a very
standard/simple backoff protocol, like in HTTP using a 503.
With queues there is never a standard way. In my experience people just don't
do flow control and end up with a huge queue which then can cause problems,
the most obvious being very high latency (like very high, like hours).

It is specially a problem if the problem you are solving is time sensitive,
like the client is waiting and will not wait forever. They will wait for like
a minute lets say and then give up. In a situation like that an ever growing
queue will just jeopardize your entire system, and it will do that in a way
that is against the idea of failing fast and being clear with users. If you
are using a task queue and the processing is done in a FIFO manner in an
overloaded system recovering from the overload will be much slower because
you will keep processing old/stale tasks (unless you implement some extra logic
o the tasks, but that adds on complexity) so new requests from clients, that would
be processed successfully in a sync system but will keep failing on the async solution
since the requests are being pushed to the back of a long queue of old requests (and the clients
who made those requests already gave up).

The topic we are getting here would be cancellation, which is something remarkably harder
to do in async systems. If your problem includes the client just giving up on what he asked
that will be quite hard to model/propagate in an async system. For async problems that include
cancellation you can't avoid the complexity, but for a simple sync problem that you
decided to model asynchornously you just bought a much bigger/more complex problem.

In some languages it is remarkably easy to deal with cancellation. In Go for example
you use a context.Context to model cancellation and that will propagate automatically,
so checking if the client is still interested on what he requested is fairly trivial,
and if the client does something like just closing a browser/app you can just stop what
you were doing, avoiding wasting resources, but more importantly freeing yourself to
process the next request (instead of having a never ending/non cancelable queue
of stale things to handle).

With request/response you can have timeouts and backoff protocols that allow you
to fail fast, with a queue you will just wait forever for an answer and never
get it or get it hours later. You can do a polling with a timeout, but it is
more complex than a simple RPC with a timeout.

What I mean is that for a scenario that request/response is a good idea,
like low latency quick requests, it is fairly clear that enforcing async on
top of it will only make things more complex, even if it is with the good
intention of failure tolerance will be better/easier.

I have seem some arguments on how sync scales less because eventually you will
hit a limit of instances (when scaling up) and those instances can only handle X
number of connections/requests and after that you will start to see failures.
That is a feature !!! What happens on a simple request/response model is that
you have a queue effect (pending requests/connections) but that queue has a clear
bounded size, if the queue is too big you start failing, and for sync problems
that is exactly what you want, there is no use to keep queueing requests internally
and answering OK to clients, they won't see a result anytime soon anyway, are they
willing to wait ? (if they are, then you have an async problem)

In the end it is a mindset. You are going to be wrong anyway when designing a system,
I just prefer to be wrong on the side of "it is way too simple and is not enough".
Specially because being way too simple is easier to detect and fix, making systems
more complex is very easy, simplifying a complex system is orders of magnitude harder.

When things are way too complicated but at least seems to be working it is
trickier to see you have a problem (or where the problem is). Because you need to detect that even
though it works, you only need a subset of the solution (and failure modes/corner cases
are specially hard to predict).

This reminds me of something said by Hoare on [The Emperor's Old Clothes](https://dl.acm.org/doi/pdf/10.1145/358549.358561):

```
I conclude that there are two ways of constructing a software design:
One way is to make it so simple that there are obviously no deficiencies
and the other way is to make it so complicated that there are no obvious deficiencies.
```

This makes it clear why it is hard to argue around complex async systems,
complex systems have no obvious deficiencies because they are so complex that
finding these deficiencies is not trivial.

But that does not mean the deficiencies are not there.
Complex systems are comfortable in a scary way.
