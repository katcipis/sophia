# Designing Data Intensive Applications

Personal notes for [this book](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321).

## Hybrid Approaches

One very interesting aspect of the book is how it defends that there
is no single magic scaling sauce, like just use this tool,
or just do this, or just use this architectural style, and you
are going to scale for sure. That never made sense to me, it is
counter intuitive since the problems play a big role on where
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

An example is addresses, more than one people can live in the exact same
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
solve all your data problems, but it kind demystifies NoSQL as some sort of
new thing that replaces SQL because SQL was stupid/complex/doesn't scale/etc.

A great read on the limitations of hierarchical structures and also on the
reasoning behind the relational model, including its objectives, can be read
on the legendary [A Relational Model of Data for Large Shared Data Banks](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf).

## Data Replication Fun

On data replication the book presents reasonably complete and good assessment of
options and problems that each approach brings, and then potential solutions,
but each with its own trade-offs. It presents these problems as fundamental
problems of distributed computing with no particular right solution, they
are intrinsic problems, specially as data gets distributed, not only computation.

Distributing computation is orders of magnitude easier than distributing data.
As you distribute data, just thinking on terms of replication, you already 
introduce some issues. If you replicate data synchronously, you defeat the
advantage of getting better scalability/latency for reads, but if you replicate
data asynchronously then you introduce all sorts of fun problems that can happen
due to replication lag.

Here is a few.

### Reads After Writes

This happens when after writing you read stale data, because the read was routed
to a replica that has lag. This has a few potential solutions, none of them
solve the problem 100%, because it is not solvable.

Shortly after a write you can always read from the leader, that will guarantee you
read fresh data, but raises the question of what would be "shortly", will the
client need a way to see how delayed read replicas are and use that to define
this time window ? It is like pushing transactions to the application layer,
something that the database could potentially try to solve, but not all databases
provide a way to do easily on replication scenarios.

Another solution would be to always read data from the leader when that data is
from a part of the system that has user specific data, that he can write. This
does work well if there is a small set of data that is user owned and lot
of data that is not user owned. The user will hardly notice stale data on data
that is not his.

Both these solutions introduce the issue that if there is a networking problem between
them and the leader/writer of the database, they lose availability. So in a
setup where you have one of fewer writers and multiple readers this makes the overall
system less available than reading from replicas all the time. You can start to
notice the trade-offs happening, and then you need to think on what makes more sense
for your system.

Another solution that is used by databases like cockroachdb and spanner is to have
a timestamp that is provided with queries and that timestamp is used as a means
to retrieve data only from replicas that are updated at least to that point in time.
This can also work with a logical clock, but the aforementioned databases do it
with actual clocks. It still not an actual solution because it may introduce
considerable latency if replicas are taking a long time to catch up and the
nodes that received the write are overloaded. Also a database that does clock
synchronization is more complex and will have trade-offs of its own. So it is not
an universal solution, but it does sound to me as the most reasonable one if you can
afford it. If you can't then you need to be more pragmatic about the level of
availability you are going to provide for different parts of the system (which is
a perfectly valid approach).

### Monotonic Reads

Monotonic reads is somewhat similar to the previous problem. It is the problem of
reading data and observe the data going back in time. The most classic problem being
reading record A and then reading it again and now it is not there. You went back in
time because one query was sent to a more updated replica, the next one goes to
a replica that is literally in the past.

An extra solution (besides the aforementioned ones) to this particular problem
is for users to always read from the same replica, and implement some mechanism
to distribute users across replicas. If you always read from the same replica then
your reads are going to be monotonic in time.

But that introduces challenges of its own, the most obvious being reduced availability
again, at least for specific users, since a failure in a single replica may
introduce failures, or being routed to a new replica that will re-introduce the
problem of going backwards in time.


### Consistency is Hard

Data consistency is hard in general. Proof of that is that even on CPU design,
when you introduce multiple parallel cores, you have all the same problems you
have in a distributed system, because a multiprocessor system is essentially
a distributed system, it is just distributed in a very small physical space
with more reliable communication, but still has all the problems regarding
sharing and consistency of data.

When faced with such challenges most architectures are considerably inconsistent
in how they handle memory. There is a good analysis about this on
[Hardware Memory Models](https://research.swtch.com/hwmm).

From the blog post:

```
Unfortunately for us as programmers, giving up strict sequential consistency
can let hardware execute programs faster, so all modern hardware deviates
in various ways from sequential consistency.
```

For me this was surprising, but one thread observing another thread writes will
not see them on sequential order, so you may observe writes in the wrong
order. And that happens even at very small scale, and it happens for the sake
of performance, because at small scale every single clock spent matters and
synchronizing memory changes would require extra effort, and not caching
things aggressively is not an option (or maybe it is, that is an interesting
open question on my mind).

### The Tale Of The Incremental Counter

As an example of the challenges that come with data replication an issue that
happened with Github is mentioned. When using MySQL an out of date follower
was promoted as leader. One table had an incremental counter as ID (instead
of some sort of UUID), and this ID was replicated on other system and used
to correlated with other data, in this specific case Redis.

What happened is that writes done on the old leader had ID X, and that ID X
was correlated on Redis for data of a specific user, when the outdated follower
assumed as leader it kept incrementing its outdated counter and it gave the same
ID to a different user. So the system showed information stored on Redis from
user X to user Y, because it re-used the same ID.

It is a tale on how failure scenarios on distributed systems can be quite complex
and also a tale of maybe always using UUIDs as IDs ? At least it would made
it much harder for this to happen (theoretically not impossible though).

## RPC

The RPC abstraction is a subject very close to my hearth, specially because I
got the end of the frenzy on remote objects, CORBA, etc. There is a renaissance
happening with RPC thanks to gRPC, so the topic is still quite interesting/relevant.

My main issue with RPC is when you use the abstraction to really try to hide that
a procedure is remote, making a remote procedure look entirely, 100% the same
as a local procedure (called local transparency).

In my opinion, and of the author, is that this is not possible.
So this part of the book is cool for me for the lame reason that it reinforces
my current opinions, and it does that with the usual good arguments around
why this sort of abstraction doesn't work.

### Different Failure Modes

Local procedures failure modes are always a subset of the failure modes of
remote ones. If a procedure A has failures modes A and B, when you make it
remote it will have those failure modes + the ones introduced by networking.

Network related failures are very different from the kind of errors you have
in a local procedure, and need to be handled in different ways. There is no
way to properly hide that without ending with a system that doesn't work
properly when faced with networking issues.

One simple example of this is idempotency issues. When you call a remote procedure
and it fails you can't safely just try again, because a failure doesn't mean a failure.
When you get a network error, maybe the procedure actually worked on the remote server,
you only failed to receive the response. So an error in this case can't be immediately
interpreted as an actual error, and trying the remote call again will depend
if the procedure is idempotent or not, if it is not now you have a very different
problem to deal with.

### Performance

The performance of remote calls is orders of magnitude slower than local calls.
In the case of networking congestion this difference can get huge. So a naive
loop that calls a procedure, that is perfectly fine if the procedure is local
will become a terrible idea if that same procedure now is made remote, and then
you need to start thinking about new trade-offs, like batching, etc.

### Overhead

Related to performance, the overhead of copying data is something that you can't avoid
on networking calls. You need serialization, transferring, de-serialization.
A series of things that would have almost 0 overhead in a local call (some stack
usage/register would be enough).

### Different Languages

Translating different data types across different languages can be quite challenging,
specially in a 100% consistent manner. So your remote procedures will never be
as simple/straightforward as remote procedures since some constructs used on local
procedures may not be supported by the remote representation, since it needs to be
interoperable with other languages. Or they may work on some odd ways, specific
to the quirks of the language.

## Isolation Levels

The book does a great job to elaborate on the broad range of isolation levels
available on databases, it does so in a way that is decoupled from specifics
and focusing on the problems that concurrency introduce in systems (even non
distributed ones).

The core issue here is how to deliver some level of isolation in a concurrent system.
In that sense the problem is not even related to databases, a lot of research on
this area is focused only on the concurrency aspect of it, you analyze it from the point
of view of multiple concurrent units send messages to the same object and how it will
behave in such a scenario. Isolation meaning how far does the system provides guarantees
for a singe client, that the single client will observe reasonable behavior.

This problem is so pervasive that it happens also on the processor level with cores/caching
introducing a lot of interesting issues regarding observed writes accross parallel cores.
The trade-offs are also the same, you usually trade performance for more coherence and more
guarantees. There is no way to have perfect/consistent behavior and not lose performance
to some synchronization mechanism, the trade-off space here is vast and complex.

Examples of isolation levels mentioned:

* Read Commited
* Snapshot Isolation
* Serializable Snapshot Isolation

Read commited is one of the more loose ones, it only guarantees that you will read anything
you commited. It won't protect you against complex problems like phantom reads. These problems
arise specially with transactions.

A transaction may read some data and make a decision based on that data, like writing X
to a database. But lets say that after it read the data, but before it writes the result, the
data it read changed under its feet, now it may be writing an invalid result because the result
is written under the assumption that the old read value is true (outdated assumption).

One simple example provided is a system were you must guarantee that at least 1 person is
on call. A transaction could consult how much people are on call, and based on that decide to remove
someone from on call duty. When two transactions are doing this, if they only have snapshot isolation,
they would cause a phatom read issue, both transactions would read that there are 2 people on call and
would decide to remove the user from on call, each of these are done on their own isolated
snapshot, and each of them write to different data, so all good at an snapshot isolation level, but
the system now entered an invalid state (no one is on call).

This particular example can be circumvented by a more elaborate transaction, different data model,
etc. But that is besides the point. The point is that the simple version of this transaction doesn't
work in a system that is non serializable.

The solution to execute such a transaction maintaining complete consistency is having
serializability on the system, which is the strongest guarantee that you can provide in a concurrent
system/database. All transactions will be executed as if they were executed serially, first transaction 1
and then transacation 2. That is the abstraction that is provided, although the implementation can vary wildly.

Implementing this efficiently is challenging and you have 2 overall families of techniques:

* Optimistic Algorithms
* Pessimistic Algorithms

### Pessimistic Algorithms

Pessimistic Algorithms as the name indicates assumes the worse, conflicts will happen
all the time so you must avoid them before they happen.

#### Just Run Serially

This seems like a joke, but this is actually how Redis work...and people love Redis =P.
It is a single thread that executes each operation sequentially. So it is a serializable
database. Of course it doesn't provide complex transactions. Actually in a database like this
the transactions must be extremelly quick/simple or else you will run into some serious performance issues.

#### 2PL Two Phase Locking

This is a very classical algorithm and was implemented on databases for decades.
It involves an initial locking phase were all locks are acquired, transaction is executed,
then all locks are released. For long transactions on the same dataset the penaly for 2PL
can be quite steep, but it can perform well under the right circumstances and it is reasonably simple.

Another major drawback of using locks this way is that systems designed like this are intrinsically
susceptible to deadlocks. Systems using 2PL need deadlock detection mechanisms and transaction aborting
to work properly.

It is specially tricky to implement on distributed systems, since the acquired/release locks
will be on multiple nodes, but it is doable.

Some people regard 2PL (and 2PC) as old/unnapropriate for distributed systems. But some modern
distributed databases like Google Spanner use them.

From [Spanner, TrueTime & The CAP Theorem](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/45855.pdf):

> To understand partitions, we need to know a little bit more about how Spanner works. As with most ACID
> databases, Spanner uses two-phase commit (2PC) and strict two-phase locking to ensure isolation and
> strong consistency. 2PC has been called the “anti-availability” protocol [Hel16] because all members must
> be up for it to work. Spanner mitigates this by having each member be a Paxos group, thus ensuring each
> 2PC “member” is highly available even if some of its Paxos participants are down.

### Optimistic Algorithms

Optimistic algorithms assumes that no conflict will happen and just execute transactions
concurrently. Then they depend on conflict detection, done after execution of transactions,
to rollback/abort any transaction that conflicted, then replaying the transaction.

As you can imagine at this point, one of the main trade-offs between the optimistic approach
versus the pessimistic one is how ofter conflicts happens. If they happen all the time the
overhead of detection + abortion + re-executing a transaction can be quite big. If they happen
sporadically then the pessimistic algorithm will keep acquiring and releasing locks for no
good reason.

There is another trade-off within optimistic algorithms, which is how fine grained the
conflict detection is. On one extreme, if you observe every single atom of data that
is read and written you will be able to be very precise when a conflict happens, avoiding
any false positives, but will have a higher overhead for every operation, since all
this bookeeping has a cost. If you observe in a more coarse grained level, like specific
tables, or just specific rows, the detection will be more lightweight but will produce more
false positives, cancelling transactions as conflicting when they actually didn't.

## Serializability and Linearizability

As soon as I started working with distributed databases some years ago I started to see
mentions of these properties but was never able to really understand them...at least
not in a simple way. Maybe Martin was the one that, finally, was able to explain that
to my limited CPU =P. Since the hallmark of trully understanding something is being
able to explain it in a simple way, I'll try to do it here. Since I don't understand the
concept that well there is no way for me to be sure if the way Martin explains it is
correct, but it did made sense and I think I understood it.

Both definitions assume that those properties belong to concurrent systems and will explain
them in terms of operations on objects, no data model is assumed.

### Serializability

This is a property of transactions, meaning that it is a property on a set of operations.
The idea of serializability is to guarantee that when you have multiple transactions you always
observe the final result as the serial execution of them in **some** order. The **some** is important
here, guaranteeing order on concurrent systems, specially distributed ones, is a really hard
problem. I won't get into ordering, partial ordering, global ordering, etc, because it is a topic
even more complex than serializability, lets just assume it is really hard so this guarantee
doesn't include ordering, just that it will be a serial execution order.

What does that mean ? lets say you have 1 object `A` and you execute 2 transactions:

```
x = read A
x += 1
write x in A
```

And:

```
x = read A
x += 2
write x in A
```

In a serializable system there are only two possible permutations on how these operations may happen.
This one:

```
x = read A
x += 1
write x in A

x = read A
x += 2
write x in A
```

Or this one:

```
x = read A
x += 2
write x in A

x = read A
x += 1
write x in A
```

Meaning that either one transaction execute first and later the other, or vice-versa, but
the operations inside each transaction can't be complected.

All other permutations would be invalid and violate serializability. Like this:

```
x = read A
x += 2
x = read A
x += 1
write x in A
write x in A
```

This example violates serializability and one of the writes is lost.


### Linearizability
