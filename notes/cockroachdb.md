# CockroachDB

## Quick Facts

* Uses ordering by key instead of consistent hashing
* Depending on sharding key, can be random (like consistent hashing)
* Uses a Raft log for consistency
* Uses one raft group per range not for the whole database (it has lot of rafts)
* Gossip is used to bootstrap raft (where to find indexes)
* Shards are called ranges
* Ranges are on the size of approximately 64mb
* If ranges are too big rebalancing can get messy
* Rebalancing is automatic
* There is no coordinator, all nodes answers queries transparently
* Works on consistency windows based on NTP clock drift window
* Nodes that get too big clock drift are removed from cluster
* No locks, optimistic concurrency is implemented, with try agains


## Architectural Layers

CockroachDB has well defined architectural layers, where the top layer only
interacts with the layer immediately bellow it. The layers are:

- SQL API
- Transaction
- Distribution
- Replication
- Storage

A brief description of each layer responsibility.

### SQL

Here the SQL statements are transformed to KV operations, where KV stands
for Key/Value.

### Transaction

This layer guarantees the ACID properties when receiving
a set of KV operations to perform (and transactions, duh).

### Distribution

The distribution layer is responsible for delivering an abstraction of a single
key/value map of data, while the map will actually be sharded on
multiple ranges across different nodes.

### Replication

This layer is responsible for replicating writes and handling the leases for
read operations, nodes without a valid lease for a read operation can't
answer a read operation for example. It will also maintain the consensus and
consistency of data that is replicated across multiple nodes.

### Storage

The final layer that is responsible for actually storing the key/value data
on disk. Right now this is done with RocksDB.

## Material

* [CockroachDB](http://cs.ulb.ac.be/public/_media/teaching/cockroachdb_2017.pdf)
* [Design](https://www.youtube.com/watch?v=p8aJuk7TJJA)
