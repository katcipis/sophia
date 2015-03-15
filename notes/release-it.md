# Introduction

The book is separated on five core concepts of writing production ready software.
Here I will try to summarize it, concept by concept. Also I will try to connect each concept.


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



# Capacity

# Design

# Transparency

# Adapt
