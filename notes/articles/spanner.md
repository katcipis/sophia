# Spanner

These are the notes from the [article](https://research.google.com/archive/spanner-osdi2012.pdf).

From the [Relational Database: A Practical Foundation for Productivity](http://delivery.acm.org/10.1145/1290000/1283937/a1981-codd.pdf) article I develop a intuition that for
some kinds of problems relational theory can serve you pretty well.

Specially if you dont get into specifics of vendors and SQL, the idea itself is
great. And a lot of momentum that NoSQL got was just because it is easier to
scale, not that the computation model is any better (even thinking about
worse/better may not make sense). Actually what is clear is that "documents" or
key/values databases where around since ever, so the renaiscance of "NoSQL" has
nothing to do with the industry having a novel idea.

It may be an expression on how we exacerbated the benefits of the relational model
and its implementation (we just used SQL for everything), or just that scaling
SQL is extremelly hard, specially with external consistency, and we surrendered
to simpler computational models. Either way there is a lot of confusion on
why NoSQL is great, like it is just simpler and better than SQL, and it is very
interesting to see the take that Google takes on the subject.

They have a lot of data intensive products, and even though some of them where
very well served with key/value databases without external consistency and
transactions, for other products that was hindering development. Solving on
the application side problems like consistent transactions is not trivial.

This became clear when they developed MegaStore and even though it scaled
poorly compared to other solutions a lot of products on Google started to use
it because of the transactions and SQL like features. This whole relational
theory and distributed transactions discussion is pretty interesting and I dont
even have the competency to argue about it, just the interest, but resuming there
are smart people on the world that is not happy with NoSQL and need SQL that scales,
here enters Spanner.

# Features

Here is a list of the features of Spanner:

* Automatically reshards data with increased data usage and when new servers are added
* Data can be replicated accross regions
* Replication is synchronous
* Data is versioned and the API externalizes this multi-version feature with timestamps
* General purpose transactions
* SQL based query language (I think today it is almost full ANSI SQL)
* Atomic Schema Updates
* Control of data locality (where data resides and colocating different tables)

As said explicitely in the article:

```
The key enabler of these properties is a new TrueTime API and its implementation
```

It does make sense to me since one of the harder problems to solve in
a distributed system is ordering of operations, and consistency on transactions
involves solving this problem well. 

The only direct contact I had with this kind of problem is
[lamport timestamps](https://en.wikipedia.org/wiki/Lamport_timestamps), which is
sometimes referred as partial ordering because it can guarantee the order of
events only when they have a causal relationship. Besides the global partial
ordering its timestamp are a monotonically increased one not an actual
wall clock timestamp. For humans usually wall clock timestamps are much
easier to reason with and more useful, but globally synchronizing time
is extremelly hard with wall clocks, but as shown in the paper not
entirely impossible (with some caveats).

Wall clock is very tricky in the sense that commodity hardware will not
be precise and even precise hardware as atomic clocks also has some drift.
Besides that there is also the problem of [leap seconds](https://en.wikipedia.org/wiki/Leap_second):

```
Earth's rotation is slowing slightly with time; thus,
a day was shorter in the past. This is due to the tidal effects the
Moon has on Earth's rotation.

Atomic clocks show that a modern day is longer by about 1.7 milliseconds
than a century ago,[1] slowly increasing the rate at which UTC is adjusted
by leap seconds. Analysis of historical astronomical records shows a slowing
trend of about 2.3 milliseconds per century since the 8th century BCE.[2]
```

So defining order using global wall clock is really hard but
at the same time extremelly useful if you find a way to solve it.

Nothing in a distributed system is 100% precise, specially because you will get
all kinds of stochastic failures, so the aim of TrueTime is to be as reliable
as the rest of the hardware that the system already depends (like the rate of
failures of CPUs compared to the rate of failure of atomic or GPS clocks).

# Organization

The first organizational unit in Spanner is the zone. A zone guarantees
physical isolation, different zones can be on the same datacenter but
never on the same machine/storage. So if you want to isolate different
applications so the data is not storared together and to distribute
resource usage better you use zones for that.

The next is the tablet, which is basically a bag of:

```
(key : string, timestamp : int64) -> value : string
```

The timestamp present with the key is important because it is what
caracterizes Spanner as a multi-version database instead of a plain
key->value database (and the other features too).

The replication unit is one paxos group. Each paxos group coordinates writes
to all replicas (with a leader/follower model that has a lease time of 10 seconds).
Reads are made directly to any node that has the data, given
that the node can safely answer the read operation given its timestamp.

On top of the tablet there is the directory (or a bucket) which represents
a set of contiguous key values. The directory is the unit of placement
on Spanner. Basically when you need to move data you do it in the unit
of directories (you can't move less than an entire directory). If the data
has to move from one paxos group to another it will be moved one directory at a time.

Directories can be moved so that directories that are accessed together
remain on the same paxos group or to make the data available on a location
that is closer to the application that is using it.

Even tough it seems that the directory will be the smaller unit since
it is the placement unit a directory still can be splited into fragments,
but for the sake of simplicity the article ignores that and elaborates
on the features in terms of directories.

# Concurrency

A good quick description of the approach:

```
At every replica that is a leader, each spanserver implements
a lock table to implement concurrency control.
The lock table contains the state for two-phase locking:
it maps ranges of keys to lock states. (Note that
having a long-lived Paxos leader is critical to efficiently
managing the lock table.)

In both Bigtable and Spanner,
we designed for long-lived transactions (for example,
for report generation, which might take on the order
of minutes), which perform poorly under optimistic concurrency
control in the presence of conflicts.
```

Locking is avoided as much as possible, only read/write transactions
require locking and the lock scope seems to be of the paxos groups
that holds the data for the given query, not a global lock, so
write transactions on different paxos groups will happen in
parallel.

# Interesting Concepts

* [Two Phase Locking](https://en.wikipedia.org/wiki/Two-phase_locking)
* [Serializability](https://en.wikipedia.org/wiki/Serializability)
* [Linearizability](https://en.wikipedia.org/wiki/Linearizability)
