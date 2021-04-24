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
But for millions, creating millions of these subscriptions and queues becomes
in itself also a bottleneck, both in cost but also in overall latency
(imagine distributing/copying millions of the same message).
