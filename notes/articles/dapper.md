# Dapper, a Large-Scale Distributed Systems Tracing Infrastructure

These are the notes from this [article](https://ai.google/research/pubs/pub36356).

# Environment

The first interesting thing on the abstract is to reflect in your
environment:

```
Modern Internet services are often implemented as complex,
large-scale distributed systems.

These applications are constructed from collections of software modules that
may be developed by different teams, perhaps in different programming
languages, and could span many thousands of machines across multiple physical
facilities.

Tools that aid in understanding system behavior and reasoning about
performance issues are invaluable in such an environment.
```

Right now (2019) there is a lot of energy around log and tracing
infrastructure, but a lot of this energy is a mix of fashion
(usual in our industry) with other mistakes. Two normal mistakes
are:

* Thinking you have the kind of environment described above, but you don't
* Building an environment like the one described above, but you don't need it

A lot of misguided microservices efforts can create an environment
where all this tooling is a good idea, and you will need the tooling,
but with a simpler non-microservicy approach perhaps you could
pass by with less, and less is more. Of course once you really work
on a big system that justifies this kind of approach, studying
Google's approach on how to tackle the problem seems like a good
idea, specially because a lot of tools (map reduce/k8s/cockroachdb)
are implementations of this kind of papers.
