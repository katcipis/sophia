# Introduction

The book is separated on five core concepts that enables production ready software.
Here I will try to summarize it, concept by concept. Also I will try to connect each concept with other stuff I know.


# Stability

A core principle is "be cynical". If anything can go wrong, it will.

Although you can't protect yourself from everything that can go wrong, you would never deliver anything.

Just give up ? No, the answer is [Recovery oriented computing](http://en.wikipedia.org/wiki/Recovery-oriented_computing).

It is not a mater of *if you will fail* but *what will happen when you fail*.

Will your entire system freeze ? Start to answer with errors to never recover again until 
a sad person goes there and restarts it automatically ?

Think about how to recover, lets see some good patterns to achieve this.


## Bulkheads

This one is pretty cool, it is a metaphor with [ships bulkheads](http://en.wikipedia.org/wiki/Bulkhead_(partition)).

Basically you partition the system by features and assign different amount of resources (like connections to a database)
to each feature.

If some partition of your system receives a lot of demand (or a DDOS) this will not cause the other partitions of 
your system to fail or get slow.

Also, a bug on one partition that causes it to consume a lot of resources (or never release a shared resource) will
not cause other partition of the system to fail (orthogonality).

Think, there is a crack on the hull of your system... will the entire system sink ?

This principle seems to play well with microservices :-).


## Timeouts

Pretty obvious stuff, every time you use something that may not respond...use a timeout.


## Circuit breaker

A pretty cool and well know pattern. If a service is not responding anymore or giving errors stop hammering it down,
wait a little to see if it will recover.

Check out [Histrix](https://github.com/Netflix/Hystrix). It plays well with the idea of not allowing cracks to spread
through the entire system (and circuit breaker is a part of this).

This is import for a microservice architecture where there will be a lot of integration between different services.
Bad stuff usually happens at the integration points.

This concept is important later on the transparency part of the book, if your system fails you must know where it 
is really failing (which one of your 10 services is the culprit :-).


## Test harness

This one is not about unit or integration testing, the author talks about a test harness to inject hell on your
application :-).

What happens when:

    * Connections gets dropped ?
    * Connection fails to handshake ?
    * Connection takes 10 minutes to handshake ?
    * An answer is received with a speed of 1 byte per minute ?

Well the list is pretty long :-), think about yourself as an evil hacker trying to break the application.

The idea is like using mocks to mess up your code, but it is recommended to use the network stack to inject the errors,
since you can't trust the libraries you are using (the idea here it to catch problems in your entire stack, not
only on your code).

It does not seem to substitute mocking, it is mocking on another level :-).
One interesting tip is to write a mock service that injects different errors when connected at 
different ports.

[Mountebank](http://www.mbtest.org/) seems to try to implement a general purpose on the wire mock, seems interesting.

The downside of this approach is that building the harness seems to be a lot harder than using traditional mocking :-).
An hybrid approach seems to be feasible.


## Fail fast

This one is pretty cool and relates to circuit breaker. Imagine you are a client, would you like to receive an error in
a matter of milliseconds or to wait ten minutes to receive the same error ?

This seems to be pretty obvious, but without good circuit breaking you will not be able to implement this.
The idea is to build on top of circuit breaking to check if you can provide a specific service.

Want a metaphor ? There is a great one :-), the [mise en place](http://en.wikipedia.org/wiki/Mise_en_place). If you 
don't have a mise en place (for example, one of the services you talk to is short circuited), just answer with an error.


### Handshaking

Application protocols like HTTP usually don't have any form of handshaking, so there is nothing preventing your
service from receiving requests when it is not able to handle it.

The idea is to implement a domain specific handshaking, so collaborating services know when to back off and stop
requesting resources.

It seems to be some kind of polite circuit breaker, don't wait for a service to stop answering or returning horrible
errors to stop hammering it.

For HTTP it seems to be a good idea to use 503 error code and a Retry-After header to do this.


# Capacity


## Myths

A lot is said about capacity, but the thing that stuck in my mind is the Myth part.
A lot of brainwash is done on the Computer Science graduation (at least on my graduation) on 
how optimization is evil and you should only be done when your system is already slow.

Specially the phrase *CPU/Memory is cheap* was said all the time. The book gives a much more healthy and
pro-active view of this. You must think about the trade-off design <-> performance.

It makes a case with two ideas (at least I remember these two :-).

First, a responsive system is essential to almost any system interacting with a human being.
People do have the same patience with slow systems as they used on the 90's :-).

Second, the multiplication factor.
Specially on the cloud this can lead to great savings on infrastructure cost. You are paying for cpu cycles.
Take a system that takes 100ms more CPU time than required (because of some kind of sloppiness). 

Well, the client is not going to notice 100ms...ok, but take a usage of 25000 requests per hour. Lets calculate how much
CPU time you are wasting (lets assume months with 30 days):

    25000 * 24 * 30 * 100 = 1800000000ms

That is *500* hours of CPU time for month, and we are talking about *one* resource of the system taking only 100ms more
than required.

I think the book does not address the idea of writing everything in assembly, this would cost more than the CPU time, 
but it takes on account the fact that developers very often are sloppy with performance.

This sloppiness is usually accompanied with a quote from Donald Knuth, usually done this way:

    Premature optimization is the root of all evil

Giving the impression that you should never consider optimizing your software before it is stupidly slow.
Well, this is just plain stupid. And I'm saying that a Donald Knuth quote is stupid, that would be also stupid :-).
The problem is how people quote it.

TODO: http://en.wikiquote.org/wiki/Donald_Knuth

A very important way to be sloppy is not measure the performance

# Design

# Transparency

# Adapt
