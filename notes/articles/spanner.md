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
at the same time extremelly useful if you find a way to do it.

Nothing in a distributed system is 100% precise, specially because you will get
all kinds of stochastic failures, so the aim of TrueTime is to be as reliable
as the rest of the hardware that the system already depends (like the rate of
failures of CPUs compared to the rate of failure of atomic/GPS clocks).

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

# Data Model

The data model of spanner is pretty interesting since it is what
they call semi-relational. They are very opnionated about the need
of SQL and transactions on the database. This opinion is not just
a hunch but really seems to be have a foundation on feedback from
other software developers:

```
 The
need to support schematized semi-relational tables and
synchronous replication is supported by the popularity
of Megastore [5]. At least 300 applications within
Google use Megastore (despite its relatively low performance)
because its data model is simpler to manage than Bigtable’s,
and because of its support for synchronous
replication across datacenters. (Bigtable only supports eventually-consistent replication across datacenters.)

Examples of well-known Google applications
that use Megastore are Gmail, Picasa, Calendar, Android
Market, and AppEngine. The need to support a SQLlike
query language in Spanner was also clear, given
the popularity of Dremel [28] as an interactive dataanalysis
tool. Finally, the lack of cross-row transactions
in Bigtable led to frequent complaints; Percolator [32]
was in part built to address this failing.
```

The point that is usually where discussion is more intense would be
the performance problems that overusing transactions brings, their
opinion is clear:

```
We believe it is better to have application programmers deal with performance
problems due to overuse of transactions as bottlenecks
arise, rather than always coding around the lack
of transactions
```

And I agree that if you need the concept of a transaction, implementing that
on the application level correctly can be pretty hard, specially with
correctness in mind =P. The best place to be seems to avoid using it
but when you need it you can rely on the database to do that.

Actually a lot of features of databases are misused because of the initial
gain you get. I have seem databases being used as message brokers and even
as a form of DNS service, it is tempting because it does solve the
problem really fast, perhaps that is why transactions also gets misused
but hardly seems like a reason to not have transactions at all on a database.

Scalability and simplicity would be better reasons to not support
transactions, and as it is usual with this kind of tradeoff making the
database simpler may leave the application code more complex (complexity
transfers from one place to the other, like the smart client/ dumb server thing).

Enough of that, another pretty cool thing is how leaking some details of the
underlying key/value storage allows applications to leverage colocation of
data, interleaving different tables together in the same directory, so if
the data is from different tables but it is always used together (with joins
for example) you can indicate that you want these data to be together in
the same directory/paxos group (which simplifies greatly locking and coordination
on write transactions):

```
Client applications declare the hierarchies in
database schemas via the INTERLEAVE IN declarations.

The table at the top of a hierarchy is a directory
table. Each row in a directory table with key K, together
with all of the rows in descendant tables that start with K
in lexicographic order, forms a directory.
```

The interleaving concepts in the end is just a appending operation
on the keys of the descendants tables. For example, given these tables:

```
CREATE TABLE Users {
    uid INT64 NOT NULL, email STRING
} PRIMARY KEY (uid), DIRECTORY;

CREATE TABLE Albums {
    uid INT64 NOT NULL, aid INT64 NOT NULL,
    name STRING
} PRIMARY KEY (uid, aid), INTERLEAVE IN PARENT Users ON DELETE CASCADE;
```

If I add a User with uid 1 I would have:

```
Users(1)
```

When I add a new album with uid 1 and aid 1 I would have this on the directory:

```
Users(1)
Albums(1, 1)
```

And if I add another album with aid 2:

```
Users(1)
Albums(1, 1)
Albums(1, 2)
```

The neat thing is that this data will reside together and contiguously
because the key is ordered lexycographically and the child table rows
just appends its own key in the parent key, so it stays ordered within
itself and it will be close to the parent table too since the
data is usually read and written together.

This is a little different from classical SQL/Relational stuff but it
seems to be a necessary adition when data is distributed geographically,
when failures and latencies are inevitable.


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


# TrueTime

The cornerstone of Spanner seems to be the TrueTime API. Besides the fact
that it relies on atomic and GPS clocks what makes it different
from commodity clocks/APIs is that it models uncertainty on the clock
explicitely, so given the interval of confidente of you timestamp you
can be closer to be sure if A has really happened before B.

Just adding the uncertainty on the API does not solve the problem,
but adding it with atomic/GPS clocks and low latency connectivity
can add up to something that is pretty reliable at least for a distributed system.

```
TrueTime explicitly represents time as a TTinterval,
which is an interval with bounded time uncertainty (unlike
standard time interfaces that give clients no notion
of uncertainty)
```

The timestamp itself seems to be pretty much like the unix one:

```
The time epoch is analogous to UNIX time with leap-second smearing.
```

On the choice of using two different kinds of clocks:

```
The underlying time references used by TrueTime
are GPS and atomic clocks. TrueTime uses two forms
of time reference because they have different failure
modes.

GPS reference-source vulnerabilities include antenna
and receiver failures, local radio interference, correlated
failures (e.g., design faults such as incorrect leapsecond
handling and spoofing), and GPS system outages.

Atomic clocks can fail in ways uncorrelated to GPS and
each other, and over long periods of time can drift significantly
due to frequency error.
```

And on the uncertainty of the clock:

```
Between synchronizations, a daemon advertises a
slowly increasing time uncertainty. 'e' is derived from
conservatively applied worst-case local clock drift. 'e' also
depends on time-master uncertainty and communication
delay to the time masters. In our production environment,
'e' is typically a sawtooth function of time, varying
from about 1 to 7 ms over each poll interval.
```

This worst case clock drift is important because it will
influence the latency of write operations directly since spanner
approach is to simply wait for this uncertainty period to pass
before commiting.

The first time I read about this was on a CockRoachLabs blog post
[Living Without Atomic Clocks](https://www.cockroachlabs.com/blog/living-without-atomic-clocks/):

```
So how does Spanner use TrueTime to provide linearizability given
that there are still inaccuracies between clocks?

It’s actually surprisingly simple. It waits.
Before a node is allowed to report that a transaction has committed,
it must wait 7ms. Because all clocks in the system are within 7ms
of each other, waiting 7ms means that no subsequent transaction may
commit at an earlier timestamp, even if the earlier transaction was
committed on a node with a clock which was fast by the maximum 7ms.

Pretty clever.
```

Which is a great read to understand the Spanner approach and also
how Cockroach approaches the same problem but using commodity
hardware (also making it clear that without the hardware support
you can't build something with the same performance and level of
consistency of Spanner).

# Schema Change

Since the original idea of relational theory was all about being
able to evolve the database for new applications without breaking
older applications sharing the same database how the schema is updated
is very important.

Schema change is applied like a transaction, but it is not a synchronous
transaction because the scope of the lock would be too big in that case
(affected paxos groups). Instead:

```
A Spanner schema-change transaction is a generally
non-blocking variant of a standard transaction. First, it
is explicitly assigned a timestamp in the future, which
is registered in the prepare phase. As a result, schema
changes across thousands of servers can complete with
minimal disruption to other concurrent activity. Second,
reads and writes, which implicitly depend on the
schema, synchronize with any registered schema-change
timestamp at time t: they may proceed if their timestamps
precede t, but they must block behind the schemachange
transaction if their timestamps are after t. Without
TrueTime, defining the schema change to happen at t
would be meaningless.
```

# TrueTime is trustworthy ?

Since TrueTime is fundamental to the whole idea of how to implement Spanner
how reliable it is ? It is easy to see that failures can still occur, but I
found interesting that empirically the clocks of the machines are more reliable
than CPUs:

```
Two questions must be answered with respect to TrueTime:
is 'e' truly a bound on clock uncertainty, and how
bad does 'e' get?

For the former, the most serious problem
would be if a local clock’s drift were greater than
200us/sec: that would break assumptions made by TrueTime.
Our machine statistics show that bad CPUs are 6
times more likely than bad clocks.

That is, clock issues
are extremely infrequent, relative to much more serious
hardware problems. As a result, we believe that TrueTime’s
implementation is as trustworthy as any other
piece of software upon which Spanner depends.
```

By observation of clock drifts it seems that even in the case of failures it is very
rare to get a uncertainty of 7ms. The 99 percentile is less than 4ms.

The results obtained with TrueTime goes as far as making bold statements as these:

```
One aspect of our design stands out: the linchpin of
Spanner’s feature set is TrueTime. We have shown that
reifying clock uncertainty in the time API makes it possible
to build distributed systems with much stronger time
semantics.

In addition, as the underlying system enforces
tighter bounds on clock uncertainty, the overhead
of the stronger semantics decreases. As a community, we
should no longer depend on loosely synchronized clocks
and weak time APIs in designing distributed algorithms.
```

# Interesting Concepts

* [Two Phase Locking](https://en.wikipedia.org/wiki/Two-phase_locking)
* [Serializability](https://en.wikipedia.org/wiki/Serializability)
* [Linearizability](https://en.wikipedia.org/wiki/Linearizability)
