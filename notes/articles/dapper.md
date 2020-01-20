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
Google's approach on how to tackle the problem seems like a good idea.

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
performance issues, although there is support for annotations
that does remind me of traditional log entries (text info for
diagnostics/humans).

Since the idea is to analyze a whole span of interactions between
different services triggered by one root request from a client the
most simple/obvious design is a tree. There should be, hopefully,
no cycles and there is always a root call. Each node is a span and
the edges is the causal relationship of caller/callee.

Each span is comprised of three IDs and basic time information:

* Trace ID
* Span ID
* Parent ID

The trace ID identifies a whole trace, that may represent a lot of
different remote calls involving different services running on different
machines. The span ID identifies a part of the trace (the node), how much time it
took to execute and if it made any other remote calls. The parent ID identifies
a causal relationship between procedure calls, if a span with ID 1 makes
a remote call, the remote call ID will be 2 and will have a parent ID of 1.

This is a very simple way to generate causal ordering on a distributed
system and it is fundamental to solve clock skew issues since the
timestamps on each span trace entry are susceptible to the underlying
machine clock (there is no special hardware as the one used on Spanner
TrueTime API).

The trace info is saved locally in a trace file, that will be collected
later for aggregation on BigTable. Both trace collection locally and the
aggregation later are subjected to different sampling configurations,
meaning that not all traces are stored for the reason of performance:

```
Low overhead was a key design goal for Dapper, since
service operators would be understandably reluctant to
deploy a new tool of yet unproven value if it had any
significant impact on performance. Moreover, we wanted
to allow developers to use the annotation API without
fear of the additional overhead.

We have also found that some classes of Web services are indeed sensitive
to instrumentation overheads. Therefore, besides making the
basic instrumentation overhead of Dapper collection as
small as possible, we further control overhead by
recording only a fraction of all traces
```

Having different sampling configurations both on the clients and on the
collector made it very easy to implement back off logic on the
collector when write speeds on BigTable drop without having to
interfere with specific clients configurations.

The implementation is very sensible on the impact that adding the
traces would cause on application performance, I find this
comforting when contrasted with ideas that usually focuses only
on benefits but ignore performance penalties and cost entirely.

# Out-of-band trace collection

They make a very good point about scenarios where out of band
trace collection is required instead of just sending the whole
trace in the response. For simple interactions just answering
with an operational trace that can be used for diagnostics
can be a very simple way to get observability (no collectors
or databases involved). But it doesn't scales very well to
long traces.

# Scaling Observability is Hard

I went for this paper because I was thinking about the whole
movement towards "logging everything structured as JSON
and storing it together" thing that is going on right now
(2019). For my surprise dapper is much more focused on
diagnosing performance issues than it is about logs
for diagnosing bugs or other kinds of system
behavior (like access logs).

It is not like it can't be used for that, it can,
quoting from paper:

```
Programmers tend to use application-specific annotations
either as a kind of distributed debug log file or
to classify traces by some application-specific feature.
```

And a concrete example:

```
The Ads Review team made extensive use of the Dapper
annotation APIs. The Guice[13] open-source AOP
framework was used to label important software components as
“@Traced.”

Traces were further annotated with information about the size
of input and output to important subroutines, status messages,
and other debugging information which would otherwise be sent
to a log file.
```

What we aim today with log aggregation is the "distributed
debug log file" thing, which in itself is a very valuable
thing, and it seems to be considerably coupled with
tracing since aggregated logs get much more useful when
you can correlate log entires (you can do this manually without
tracing, sometimes). If logs of different
services are not going to be correlated you can just
get them directly from running services instead of
putting it all together on the same database for searching.

I think this is mentioned as a secondary sparse usage
of tracing because scaling a distributed debug log
file does not seem possible for high performance systems
and for the ones where performance is not an issue
cost will probably be. This feeling comes from
all the hardships they had to scale a very simple
trace infrastructure that deals with binary messages
that usually have only a few ID's and timestamps and
limits on the size of string annotations (I have seem
my share of gigantic JSON log entries).

For example, storage wise:

```
Our production clusters presently generate more than
1 terabyte of sampled trace data per day.
Dapper users would like trace data to remain available
for at least two weeks after it was initially logged from
a production process.

The benefits of increased trace data density must
then be weighed against the cost of machines and disk
storage for the Dapper repositories.
```

And this is for very minimal tracing data. Imagine what
would be the numbers for aggregated structured logs as
JSON (current industry best practice AFAIK).
It is not that having observability at this level
would not be useful, it is invaluable for diagnostics, but such
a system does not seem to be scalable (cost and performance wise).

You can have the distributed logs and analyze each one of them
individually (scales linearly), but collecting everything and putting
together for correlation, even tough it is very useful,
does not seem to be practical
at scale. Even a simple info log entry per request, which
would be the minimal for it to be useful on diagnostics,
would seem to generate more overhead than dapper,
both on the client (marshalling JSON) and on storage (storing JSON).

The best you could do would be to have a single log entry per error
with all the relevant information on it, that then will be serialized
as JSON and collected. This would cause sampling since hopefully your
application is not generating errors all the time and make it
much more easier to scale collection and storage, but the single log
entry needs to be carefully crafted to be useful, something that
I seldomly see in code (or do it myself sadly). And even in that
happy scenario I'm not sure how far it would scale anyway, but it has
the best shot. Perhaps that is the idea of the annotation on the trace,
carefully crafted information to aid diagnostics, instead of the
usual shower of log info calls that usually is present on code.
But on dapper it is added on a binary protocol with a limit on
the annotation size, instead of arbitrarily big JSON entries.

The most interesting realization for me is that just collecting
all logs and putting them together is not a modern cloud native awesome
way to get observability. Actually it is how to build a system
that does not scale because of the cost of this observability.
And even if you don't need to scale, you are paying for all
this infrastructure. Of course you need observability to
diagnose production problems, but it seems like a very lazy
and brute force approach that is presented as the greatest idea ever in
distributed systems and that everyone should do it (because company X does it),
and this paper kinda consolidated for me that this notion is
just plain wrong (just the notion, sometimes brute force is great).

Another severe bottleneck is the database that will ingest
all this data. In the case of Google, even using BigTable there are
still reasonable concerns around performance when
writing all the traces, and indexing/searching is quite limited
so the system can scale. Then again, if using BigTable with very few
indexes is already a problem at scale, the idea that you are going to
dump a plethora of JSONs and search by any field at scale does not
seem feasible, and if it is it definitely does not seem cheap.
The most common database used for this scenario is ElasticSearch,
I have seen it not scaling, specially when you need a lot of indexes
for a different fields in a dynamic nature, adding nodes solved nothing,
and even if in your scenario adding nodes solves it, the nodes will not
be cheap (Elastic Search loves memory).

Sometimes you can just do it and it will work for you in your context
(as brute force usually does), the scale is not big,
the cost does not seem prohibitive, and that is
OK, but don't think it is a cloud awesome scalable way to solve
the observability problem, it should not be presented like that, but I suppose
that Cloud Vendors and Database Vendors are very interested on this
idea to be presented as a panacea of observability =).
