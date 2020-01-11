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
(usual in our industry) with other mistakes. Two normal mistakes are:

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

# Ubiquity

The first important requirement of a distributed tracing system is
ubiquity, since the idea is to have full visibility of the path a request
takes inside a reasonably complex system if a single service does
not propagate tracing information correctly you will not have
an overall accurate vision of what is happening on the system.

This does not mean that you can't implement this in incremental steps,
but to be really useful to performance analysis and debugging it needs
to be a complete view of the system.

Ubiquity introduces the problem on how to add tracing to a system
(everyone has to do it), through instrumentation or in a
full black box manner ?

# Black Box VS Instrumented

I have a little experience with this kind of tradeoff on the monitoring
landspace. For example, you can have http response times metrics by instrumenting
your code, like you do with [Prometheus](https://prometheus.io/),
or with a black box approach like you can do with [Sysdig](https://sysdig.com/).

In my opinion, when you can go with black box you should go with black box
because it gives you ubiquity with no application level effort.
Since Sysdig uses eBPF to extract metrics directly from the kernel all services
running on top of the Kernel will get the same metrics consistently,
if you use instrumentation or even user space proxies (like Linkerd/Istio)
it is easier to not cover part of the system since instrumentation requires
collaboration from application level developers or because there are more
moving parts to run/configure (side cars).

Getting back to the paper itself, they are explicit about the downside of
instrumentation, and avoiding it was one of the requirements:

```
A tracing infrastructure that relies on active
collaboration from application-level developers in order
to function becomes extremely fragile, and is often
broken due to instrumentation bugs or omissions,
therefore violating the ubiquity requirement. This
is especially important in a fast-paced development
environment such as ours.
```

Funny enough, in the end they do rely on instrumentation, but
that is done on core libraries that are consistently used across all Google:

```
Our instrumentation is restricted to a
low enough level in the software stack that even largescale distributed
systems like Google web search could
be traced without additional annotations. Although this
is easier to achieve since our deployment environment is
blessed with a certain degree of homogeneity
```

So it feels like they kinda cheated but in a very pragmatic way that is
very common from Google =). Basically they achieved the "no instrumentation
required from devs" through instrumentation on the RPC library that is used
ubiquitously. You have instrumentation, but not done manually by all developers
on all services, it seems like a win for consistent environments and good enough
solutions, it does not cover the small percent of cases where the communication
is not done through RPC, but it is good enough.

It is not uncommon to find people in the industry, working in much smaller scale,
that are much more pedantic with perfect solutions or being extreme with ideas
like never sharing logic through libraries (extreme microservices). Google operates
at fantastic scale with no regard with such rules. Understand the requirements well
and define good enough solutions and that is it.

But since black box tracing monitoring can ensure ubiquity better and decoupled
from build/libs constraints, why it is not used ? The problem is that on tracing
you need correlation between different network interactions, so you can know that
request 1 spanned other 3 requests, etc. Doing that in a black box manner is not
impossible but it is not deterministic, from the paper:

```
Two classes of solutions have been proposed to aggregate this information so
that one can associate all record entries with a given initiator
(e.g., RequestX in Figure 1), black-box and annotation-based monitoring
schemes. Black-box schemes [1, 15, 2] assume there is
no additional information other than the message record
described above, and use statistical regression techniques
to infer that association.

Annotation-based schemes [3, 12, 9, 16] rely on applications or middleware to
explicitly tag every record with a global identifier that
links these message records back to the originating request.

While black-box schemes are more portable than
annotation-based methods, they need more data in order
to gain sufficient accuracy due to their reliance on statistical inference.
```

Given the accuracy issues + the opportunity to leverage shared code for
instrumentation they ended up with shared code instrumentation. It is interesting
that the level of consistency is quite high on Google services:

```
In our environment, since all applications use the
same threading model, control flow and RPC system, we
found that it was possible to restrict instrumentation to
a small set of common libraries, and achieve a monitoring
system that is effectively transparent to application developers
```

# Overall Design

The design itself seems quite simple and focused on diagnosing
performance issues. It is comprised of three IDs:

* Trace ID
* Span ID
* Parent ID

The trace ID identifies a whole trace, that may represent a lot of
different remote calls involving different services running on different
machines. The span ID identifies a part of the trace, how much time it
took to execute and if it made any other remote calls. The parent ID identifies
a causal relationship between procedure calls, if a span with ID 1 makes
a remote call, the remote call ID will be 2 and will have a parent ID of 1.
This is a very simple way to generate causal ordering on a distributed
system and it is fundamental to understand the spanned remote calls of
the root call made by the client and how much time each one of those
took to execute.

The overall design of the solution (local trace file/collectors/bigtable
sparse tables)
Interesting to have sampling on trace generation and on collection/writing.

# Annotations

Talk about logging/debugging info appended to traces.

# Scalability issues

The hardships to scale such a massive
