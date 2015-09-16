# I lov logs


## Introduction

Logs are presented as a data structure, an array of records, ordered by a timestamp.
The timestamp (and a way to provide them) is a important concept, specially on distributed system.

You can model two ways to replicate state using the idea of logs. Master / Slave Backup 
and State Machine Replication.


### Master / Slave Backup

On this case you keep a log of the state changes, and you are able to reproduce state from the
master to the slaves.

The master receives read/writes and generate the logs that are consumed by the slaves.
Slaves can assume at anytime, thanks to the state upgrades provided by the log.

Example:

    x = 1
    y = 3
    x = 7


### State machine replication

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


## Consensus Algorithms

* Raft
* ZAP
* Viewstamped replication


## Ideas

### Rastreability

Besides correlation ids we could use something like [lamport timestamps](https://en.wikipedia.org/wiki/Lamport_timestamps)
so we could achieve correlation and partial ordering of events :-)

## TODO

Stopped at pag 23
