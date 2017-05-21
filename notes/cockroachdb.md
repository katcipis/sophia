# CockroachDB

## Quick Facts

* Uses ordering by key instead of consistent hashing
* Depending on sharding key, can be random (like consistent hashing)
* Uses a Raft log for consistency
* Gossip is used to bootstrap raft (where to find indexes)
* Shards are called ranges
* Ranges are on the size of approximately 64mb
* If ranges are too big rebalancing can get messy
* Rebalancing is automatic
* There is no coordenator, all nodes answers queries transparently
* Works on consistency windows based on NTP clock drift window
* Nodes that get too big clock drift are removed from cluster
* No locks, optmimistic concurrency is implemented, with try agains

## Material

* [Design](https://www.youtube.com/watch?v=p8aJuk7TJJA)
