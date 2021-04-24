# Designing Data Intensive Applications

Personal notes for [this book](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321).

## Hybrid Approaches

One very interesting aspect of the book is how it defends that there
is no single magic scaling sauce, like just use this tool,
or just do this, or just use this architectural style, and you
are going to scale for sure. That never made sense to me, it is
counter intuitive since the problems pay a big hole on where
bottlenecks will be present and each bottleneck can be quite
challenging to solve, given the specific constraints of the problem at hand.

But I don't have that much experience, so it is always good to see someone
else with more experience reinforcing the idea that there are no
easy answers, specially when there are so much "thought leaders"
selling those.

And he presents this idea with real world examples, in this case Twitter.
He navigates how Twitter evolved over the years to scale. In the case of
Twitter the main feature that imposes scaling challenges is the follower/
followee relationship, where one person can post something and that needs
to trigger an event to maybe millions of people. In a system like that
writing is initially not a problem, but the amplification of a single write
to millions of reads imposes a challenge.

At the very beginning, as it is expected, they started with something very
simple/plain. Just a database + request/response model. It scaled well for
quite some time, but it started to suffer as they had more and more users.
Because of the followers/followee relationship it feels very natural to model
the system as a Pub/Sub system, when someone posts something it pushes an event
and then all subscribers (followers) receive that event. That way each
subscriber can have its own cache of queued up messages that is independent, no
single point of access, the miracle of scaling :D.

Except that for people who have millions of followers that doesn't scale.
It scales brilliantly for people with hundreds and thousands of followers.
But for millions, creating millions of these subscriptions and queues/caches
becomes in itself also a bottleneck, both in cost but also in overall latency
(imagine distributing/copying millions of the same message).

So what to do ? An hybrid approach were for the average use case Pub/Sub is
used and for people with millions of followers the good and old request/response
model is used, and the requisitions on the database are done as users open
their accounts to receive the feed, to avoid millions of simultaneous subscriptions
hammering down the database.

In most cases you have your own cache of tweets, and for some special cases
it may take a little more time to receive that tweet from a very famous person.

I found that to be a formidable example of how good engineering is usually
hybrid and is only achieved when you focus on the actual problem and the
constraints it imposes instead of following architectural patterns
as a religion for miraculous promises of eternal scaling.

This also reminds me a lot of the dissertation [Architectural Styles and
the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
where REST is defined and a great emphasis is given on not focusing on a single
architectural style as an answer to all problems, because there is no such thing.
It also provides some really cool analysis of different architectural styles,
and a whole framework on how to think about them and compare them. Not to
find a winner, but to understand which one is applicable to which kind of
problems. And as the whole Twitter system teaches, you probably won't be able
to build something scalable with a single architectural style, you need to
know a lot of them and compose them together.

All this also reminds me of the design principle of
[Form Follows Function](https://en.wikipedia.org/wiki/Form_follows_function).

## Data Models

The book is also brilliant on how it presents different data models,
like comparing SQL/NoSQL, because it also avoids finding "winners" and it
also provides a lot of historical context, specially in detailing that document
databases are not a new thing, they are actually older than relational databases
and they just made a comeback because they are much easier to scale,since the
data model is simpler.

Well, I mean, document databases also make some things easier, like if you always
retrieved data aggregated in the exact same way a document database will be faster
and will feel much simpler, because it is. Document databases explore locality
better. If you always need the very same tree of data, you are golden with
documents. The hierarchical model represents well one to many relationships,
like all information related to a user in the same document. The problem is when
you start having many to many or many to one relationships.

An example is addresses, more than one people can leave on the exact same
apartment, how to model that many to one relationship in a way that avoids
duplication and is consistent and easy to maintain ? 

Here enters the relational model, exactly to model that. You could argue that
we need better models than the relational model, but an hierarchical model
is not new neither more expressive, it is a simpler model suited for simpler
problems, and it was well know by the people who developed an alternative
solution, to when you problem domain demands something more expressive.

I also find remarkable how the whole idea of the relational model is to
enforce decoupling. They wanted to decoupled programs not just form the internal
representation of the data, but also from external presentation, so even published
schemas should be able to change and be extended without requiring changes
on programs, and yet a lot of people present SQL databases as some sort of
nightmare to evolve, it does give me a deep sense of "we are doing something
terribly wrong with our data models", but it is not the relational model that is
at fault, and an hierarchical model will definitely not help you much if
your problem is "complex data with N-N/N-1 relations".

Also even though the hierarchical model makes it easier to load aggregates
of data, it does couple the application reading the data on specific decisions on
how the data is hierarchically laid out, something that the relational model
avoids by design, since it was a well know problem to when you have a hard
problem that evolves along time and then you figure out that the hierarchy
that you choose makes no sense (and you can't change it without breaking
all programs using that data).

But again the book presents the idea that there is no silver bullet that will
solve all your data problems, but it kind dis-mystifies NoSQL as some sort of
new thing that replaces SQL because SQL was stupid/complex/doesn't scale/etc.

A great read on the limitations of hierarchical structures and also on the
reasoning behind the relational model, including its objectives, can be read
on the legendary [A Relational Model of Data for Large Shared Data Banks](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf).
