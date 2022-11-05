# Software Architecture

Some material/ideas I found along the way on software architecture.
Also some personal rants.


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
(the web is basically request/response, so the argument doesn't make sense).

But lets focus on the ways that async/queues makes things more complex, because
they do, they may provide benefits but the costs must be clear, since people
usually focus on benefits I will focus on the costs :-).

//DRAFT

well, by definition it is harder to code around that + you have more failing points, instead of service A + service B you now have service A + broker + service B, that is by definiton more moving parts and more things that can go wrong

the code itself is also harder, on the client side you need to send something on a queue and now you have to find a way to wait for an answer

because in a lot of scenarios it envolves waiting for an answer with queues

and now you need some polling

while with req/response it is literraly one line, you send request...and get answer right away...you can't beat req/response and RPC because nothing can be simpler than a single function call. You would need to create a function that does something similar but does the polling watining for the answer..which is usually something that is not standard, so you need to reimlpement this polling over and over again

and you need some way to correlate the answer, so now you need a job/req ID, manually handled...and if that colides you are fucked (but it is reasonably easy to avoid colision..but it still is more to worry about

+ flow control is more complex....in req/resp you can have a very standard/simple backoff protocol, or in HTTP also leverage 503. With queues there is never a standard way....in my experience people just dont do flolw control and end up with a huge queue which then can cause problems. It is specially a problem if the problem you are solving is time sensitive..like the client is waiting and will not wait forever...he will wait for like a minute lets say and then give up..in a situation like that an ever growing queue will just fuck up your entire system, and it will do that in a way that is against the idea of failing fast. WIth req/resp you can have timeouts and backoff/resp codes that allow you to fail fast, with a queue you will just wait forever for an answer and never get it. You can do a polling with a timeout...but it is more complex than a simple RPC with a timeout

+ after the timeout happens and you give up...now you have the problem of answers to requests on the queue that are orphans, because the user already gave up on them...what to do with them ? something needs to drain them. All of those are no problems on req/resp...if you give up on a request you cancel the connection/stream and the other side will log an error and that is it...simple and beautiful.

what I mean is that...for a scenario that req/resp is a good idea, like low latency quick requests...it is fairly clear that enforcing async on top of it will only make things more complex.

but of course there are more subtle scenarios, that lend themselves more easily to async, then the discussion can be more rich.

for example, in a banking system, transfer for sure are async..because the real world problem they solve literally can take days. So something async is very suitable

although...you can also do async with simple REST and postgresql, you dont need anything fancy..for simple scenarios. You can have a service that keeps state about the operation internally and then some polling mechanism (which is inevitable).

the single service/database approach wont scale as well as some fancier solutions, but will be easier to maintan and will be more consistent, if it is enough for your problem domain

in the end it is a mindset...you are going to be wrong anyway...I just prefer to be wrong on the side of "it is way too simple and is not enough". Specially because being way too simple is easier to detect. When things are way too complicated but at least seems to be enough it is trickier to see you have a problem. Because you need to detect that even though it works, ou only need a subset of the solution.

ah just reminded one quote from Hoare:

"I conclude that there are two ways of constructing a software design: One way is to make it so simple that there are obviously no deficiencies and the other way is to make it so complicated that there are no obvious deficiencies."

it makes it clear why it is hard to argue around the complex async systems, complex systems have no obvious deficiencies because they are so complex that finding them is not trivial. But that does not mean the deficiencies are not there =P. So complex systems are comfortable in an scary way.

//DRAFT

## Procedures inside the database ?

* Putting procedures inside the database is the utmost level of coupling with the database
* It is a tradeoff from latency to strong coupling, you are coupled to the technology now
* If you need to change the database for some reason, you are going to have a bad time (eg. Neoway)
