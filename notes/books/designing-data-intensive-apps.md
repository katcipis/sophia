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

Except that for people who have millions of followers..that doesn't scale.
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
