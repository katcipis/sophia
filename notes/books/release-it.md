# Introduction

The book is separated on several core concepts that enables production ready software.
Here I will try to summarize some of then, concept by concept. 

Also I will try to connect each concept with other stuff I know.


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
Also, handle the timeout and release any resources associated with the timeout.


## Circuit breaker

A pretty cool and well know pattern. If a service is not responding anymore or giving errors stop hammering it down,
wait a little to see if it will recover.

Check out [Histrix](https://github.com/Netflix/Hystrix). It plays well with the idea of not allowing cracks to spread
through the entire system (and circuit breaker is a part of this).

This is import for a microservice architecture where there will be a lot of integration between different services.
Bad stuff usually happens at the integration points.

This concept is important later on the transparency part of the book, if your system fails you must know where it 
is really failing (which one of your 10 services is the culprit :-).

Without a proper implementation of something like a circuit breaker, when you analyse a log it will look like 
all your services are broken, because the erros will spread. The idea of circuit breaking is to service X log something
like *I'm not working, but it because Y is not working* instead of simple exceptions/errors exploding.


## Test harness

This one is not about unit or integration testing, the author talks about a test harness to inject hell on your
application :-). It is interesting that he uses the Test Harness expression, that is very common on unit testing.

The idea is to think about what happens when:

* Connections gets dropped ?
* Connection fails to handshake ?
* Connection takes 10 minutes to handshake ?
* An answer is received with a speed of 1 byte per minute ?

Well the list is pretty long :-), think about yourself as an evil hacker trying to break the application.

The idea is like using mocks to mess up your code, but it is recommended to use the full network stack to inject the 
errors, since you can't trust the libraries you are using (the idea here it to catch problems in your entire stack, not
only on your code).

It does not seem to substitute mocking, it is mocking on another level :-).
One interesting tip is to write a mock service that injects different errors when connected at 
different ports.

An interesting implementation of this idea is [toxiproxy](https://github.com/shopify/toxiproxy). There is also
[Mountebank](http://www.mbtest.org/), a general purpose on the wire mock.

The downside of this approach is that building the harness seems to be a harder than using traditional mocking :-).
An hybrid approach seems to be feasible.


## Fail fast

This one is pretty cool and relates to circuit breaker. Imagine you are a client, would you like to receive an error in
a matter of milliseconds or to wait ten minutes to receive the same error ?

This seems to be pretty obvious, but without good circuit breaking you will not be able to implement this.
The idea is to build on top of circuit breaking to check if you can provide a specific service.

Want a metaphor ? There is a great one :-), the [mise en place](http://en.wikipedia.org/wiki/Mise_en_place). If you 
don't have a mise en place (for example, one of the services you talk to is short circuited), just answer with an error 
immediately. You have the advantage of being able to give a pretty good error message.


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
People don't have the same patience with slow systems as they used on the 90's :-).

Second, the multiplication factor.

Specially on the cloud this can lead to great savings on infrastructure cost. You are paying for cpu cycles.
Take a system that takes 100ms more CPU time than required (because of some kind of sloppiness). 

Well, the client is not going to notice 100ms...ok, but take a usage of 25000 requests per hour. Lets calculate how much
CPU time you are wasting (lets assume months with 30 days and a 24 x 7 service):

    25000 * 24 * 30 * 100 = 1800000000ms

That is *500* hours of CPU time for month, and we are talking about *one* resource of the system taking only 100ms more
than required.

I think the book does not address the idea of writing everything in assembly, this would cost more than the CPU time, 
but it takes on account the fact that developers very often are sloppy with performance.

This sloppiness is usually accompanied with a quote from Donald Knuth, usually done this way:

    Premature optimization is the root of all evil

Giving the impression that you should never consider optimizing your software before it is stupidly slow.
Well, this is just plain stupid. And I'm saying that a Donald Knuth quote is stupid, that would be also stupid :-).
The problem is how people quote it, here is the full phrase (from the [wiki](http://en.wikiquote.org/wiki/Donald_Knuth)):

    Programmers waste enormous amounts of time thinking about, or worrying about, the speed of noncritical parts of 
    their programs, and these attempts at efficiency actually have a strong negative impact when debugging and 
    maintenance are considered. We should forget about small efficiencies, say about 97% of the time: 
    premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.

You can see clearly that he was talking about optimizing everything like crazy, even stuff that would not matter.
Because of this idea I see a lot of people that has no regards to performance and optimization, they don't monitor
anything, and if they do and see a lot of CPU and memory usage they simply don't care, well, it works right ?

I'm totally ok with the concept of working software first, but there is a balance, specially because even on the start of
the project you can make choices that will help you have a faster and cheaper system.

* Am I going to use that gigantic framework to do some work that I need and also a lot of more crap ?
* Why use a language that uses a lot more resources if this other one uses a lot less resources ?

Of course there is a balance, probably you will not chose C to develop services because it is small and fast, the
level of abstraction is too low. But this kind of mindset is what is making tools like [Golang](https://golang.org/)
get a lot of traction.

You have a good balance between ease of developing, good tooling, but great performance. And yes, thinking about
performance from the beginning of the project is not completely wrong, just don't let it stop you from delivering software
soon.

Bandwidth is also expensive, so avoid chatty protocols :-) (there it goes remote objects, designing remote objects
as local objects will probably result on a chatty protocol and isolating the developer from this reality is *NOT* a
good idea).

A very important way to be sloppy is not measure the performance. And this is covered on the *transparency* concept.


# Transparency

One of the main things that stuck on my mind on the transparency concept is
this great mechanical metaphor, like they can know what is wrong with the
engine because of the noise :-).

Does your code make noises ? How do you listen to it ?

The metaphor makes it very clear that the software have to make noise,
but by default it does very little noise. This noise would be perceived by
monitoring the host, stuff like I/O usage, CPU usage, memory usage, etc.
Basically any kind of noise you can hear that does not comes directly from
the application on a domain level.

Comparing to testing, it would be like black box monitoring, you are
monitoring the application from the outside.
This is very important but really far from what is proper transparency.

Proper transparency gives you domain level information about what is
happening on your system. Not only for troubleshooting, but even to know how
the user interacts with the system.

Some good advices:

* Monitor capacity continuously. Each application release can affect scalability and performance.
* Have a feedback process that acts responsively to meaningful data. Don't generate useless reports that no one sees.
* Use this feedback on a [OODA](https://en.wikipedia.org/wiki/OODA_loop) like loop.
* Do you have traceability on your system ? Like an ID that allows the steps of a request to be traced on the system

The [OODA](https://en.wikipedia.org/wiki/OODA_loop) thing is very interesting,
it comes from the military and it is based on the idea that on the battlefield
you cant foreseen everything and the most important thing is to
observe and adapt, a concept that is very well know to anyone interested on
agile software development.

Besides the concepts there is also the tooling involved on aggregating and
visualizing all the data (also setting some alarms). There is a lot of
information on this subject, so it will not be covered here
(just as it is not covered on the book).

The acceleration on this form of feedback is even changing the shape of
how people see testing and practices like
[TDD](https://en.wikipedia.org/wiki/Test-driven_development).

Independent of the nature of your system, transparency makes the difference
between a system that improves over time in production and one that
stagnates or decays.

Good data enables good decision making.


## Alerting

The book has very little details on how to setup monitoring and alerting.
It only makes a case of giving it thought as a first class citizen problem
to be solved.

There is this
[doc on alerting](https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit#heading=h.fs3knmjt7fjy)
that has pretty good advices on alerting.


# Adapt

This is the last part of the book and it asses how your service will adapt the inevitable
changes. Obviously it mentions [Extreme Programming](http://www.extremeprogramming.org/) 
since it is about embracing change :-).

The core concept is that no matter how bold the vision or how severe the crunch mode, the system
that launches will always be less than it might have been.

So you must focus some energy on how your software will be able to the *required* adaptations.
On this regards he gives a great deal of importance to unit testing and refactoring.

Refactoring obviously is necessary to improve the design of the system as it needs to adapt.
The tests should allow you to have a very fast feedback loop to check if you broke something.
It is that simple, of course that properly implementing this idea is very domain specific and
very hard (at least for me :-).

But it is fundamental to evolve a system. One part that I have to quote entirely is this one:

    Any strategy formulated predicated on creating a monoculture - whether it is a single integration
    technology or a single programming language - is doomed to be a costly failure

This is *very* sound advice :-). Of course that when the subject is integration technologies it is
a good idea to remain technology agnostic and avoid having a dozen of different technologies
(nobody wants to use HTTP + protocol buffers + RPC + SOAP just to get one thing done).

But going to the extreme of using only one is definitely a bad idea.

The language thing is pretty obvious :-), and there is also the concept of 
[polyglot persistence](http://martinfowler.com/bliki/PolyglotPersistence.html), which makes perfect sense
(although it has some serious trade-offs when the subject is consistency and duplication of data).

Since we are talking about persistence, the author gives serious advice to *NEVER* do database 
integration. Not even with stored procedures. Always wrap up services around it.

There is a lot of reasons for this kind of advice, the 
[Building Microservices](http://info.thoughtworks.com/building-microservices-book) explains some of then,
like lack of cohesion, excessive coupling and all that nasty stuff that does not allow software to
adapt and evolve properly.

But one that is very simple is:

    A system that hangs its database out for the world cannot trust the data in the database at all

I agree that it is that simple, the only thing that prevents catastrophe on a system like that is pure fear
and the absolute certainty that no one can change the database, which goes completely against the idea of
adaptation to a changing environment. It is a basic notion of encapsulation and avoiding global state.

The only reason I can think that this is done that much it is because it is much easier than defining proper
domains for each service and good integration protocols. It is faster on the beginning, but it wont scale
(neither in performance terms or engineering effort terms).

Since the only way to adapt is to adapt to some kind of feedback you need to make releases as easy as
taking a haircut. Here the TDD or even QA feedback won't cut it, you want to adapt to the market, you need
feedback directly from the client.

The faster you get that feedback loop to operate, the more accurate those improvements will be.
This demands frequent releases and good transparency. This makes transparency even more important than
testing (it does not give you a free pass to testing although :-)), without it it is very hard to
release frequently and it is impossible to have fast feedback.


# Conclusion

The book beautifully concludes with the idea that code must be flexible and adapt to changes,
and that this is only proved when it is already on production.

And that is the only measure of a system quality, besides talking about a lot of patterns it
says clearly that evaluating a system by *count of patterns applied* is *never* a good quality
metric.

The only good quality metric is the how happy your client is, and how much is costing you to keep him happy.
