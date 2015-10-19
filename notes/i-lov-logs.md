# I lov logs


## Introduction

Logs are presented as a data structure, an array of records, ordered by a timestamp.
The timestamp (and a way to provide them) is a important concept, specially on distributed system.

You can model two ways to replicate state using the idea of logs. Master / Slave Backup 
and State Machine Replication.


## Master / Slave Backup

On this case you keep a log of the state changes, and you are able to reproduce state from the
master to the slaves.

The master receives read/writes and generate the logs that are consumed by the slaves.
Slaves can assume at anytime, thanks to the state upgrades provided by the log.

Example:

    x = 1
    y = 3
    x = 7


## State machine replication

Here you keep a log of all the events that happened on the system.
Given a deterministic state machine, if you provide the same input to it, it will
achieve the same state, independent of how much times you execute it.

If you have a DSM, them you can use the log of events to reproduce the DSM and achieve
the same state.

On this model the state machines are peers, there is no master / slave.

Example:

    x = 0
    y = 0
    x += 2
    y *= 4
    x = x + y

It would be analog as a bank log of operations (debits and credits) that are used to
calculate the current state of your account


## Organizational Scalability

Any team can publish on the feed and be responsible for the published data.
Avoid a central ETL team, they will become a bottleneck and wont be able to scale.

These ideas is analogous to the devops culture of each team owning its own service + deployment,
without a central team responsible for a part of the system that is essential to features rolling out
to the client.

On the devops side, this would be operational stuff related to deploying stuff. On data integration, would be
the idea of publishing data. Even if you have a team responsible for aggregation and cleaning and doing awesome
stuff with data, it should not be **mandatory** that any data available on the system must pass through them.

The event feed is the central API for publishing and retrieving data, but there is no central team.
Defining and respecting protocols is important, and seems that [Postel Law](https://en.wikipedia.org/wiki/Robustness_principle)
is specially useful here, since it will make integration and evolution of the protocol a lot easier.

This central log is very simple (assume as less structure as possible), and supports real time + batch processing gracefully,
not assuming anything on how the data is used. This reminds greatly of the [data lake](http://martinfowler.com/bliki/DataLake.html)
concept, opposing to the traditional data warehouse. On this case, the basic structure could be a JSON, but there is no
global assumption about the structure, the contract is between each consumer <-> producer.


### Scaling the log

If everyone is going to integrate through the log then this log has to scale horizontally and have decent
consistency (eventual, but decent :-).

Cool stuff:

* Sharding
* Replication
* Key partitioning

Sharding and replication is well know, key partitioning is the idea to replicate and shard keys independently,
this way keys are completely orthogonal and you can add new keys indefinitely without interfering on existing
keys (you never get full orthogonality, but it is a good goal).


## Good properties of a event log

* Favor N consumers (and scales to it)
* Persists the events
* Enable reprocessing of past events (important to failure modes and bug fixes)
* Lifetime of events can be configured in time or space


### Technologies

A list of technologies that provides these nice properties :-)

* Kafka


## Consensus Algorithms

* Raft
* ZAP
* Viewstamped replication


## Ideas

### Rastreability

Besides correlation ids we could use something like [lamport timestamps](https://en.wikipedia.org/wiki/Lamport_timestamps)
so we could achieve correlation and partial ordering of events :-)
