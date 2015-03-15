

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

# Capacity

# Design

# Transparency

# Adapt
