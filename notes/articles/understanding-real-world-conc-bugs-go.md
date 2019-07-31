# Understanding Real-World Concurrency Bugs in Go

The original paper: [Understanding Real-World Concurrency Bugs in Go](https://songlh.github.io/paper/go-study.pdf)

# Idea of the research

The idea is to find good samples of open source Go project and by empirical
analysis understand the nature of concurrency bugs in Go. To answer questions
like, are channels and message passing safer and easier to use than mutexes ?

The paper is considerably honest in admitting how hard it is to come
to conclusion and its limitation. You are in the land of extreme non-determinism.
You are doing a manual sample, analyzing it empirically and the bugs
that you are analyzing are themselves non-deterministic, it is hell and yet
someone needs to at least try.

The projects chosen are very interesting ones, like Docker, Kubernetes,
etcd, boltdb, etc. They include databases and cluster orchestration systems,
and all the projects have more than 4 years of life and are of considerable
size.

And yet, since it is a very limited sample, the conclusions that they get
at the paper are somewhat too bold, like saying that channels and messaging
passing is not more clear or safer than just synchronizing shared memory, just
because on some cases they produce more bugs. The sample is too small and there
is a lot of variables that are hard to measure, for example perhaps channels
are only used on more complicated algorithms, and hence bugs appear more there.
The same algorithm, without channels, would have less, the same or more bugs
using shared memory ? This is not easy to measure since it extrapolates
on how people in general think and make mistakes.

Even tough, they make a lot of bold claims along the
article, some of the conclusions are really interesting, like space for
better tooling detecting simple concurrency errors using channels.


# To be concurrent or to not be concurrent

TODO: my personal experience with concurrency in Go, if you can avoid it do it.
chans and messages are good tools, they may make it easier to understand concurrent
algorithms, but easier is not the same as easy, sequential is still easier
than concurrent (bad tools or bad brains ? or both ?)

Making it easier has the tradeoff of abusing, this reminds me of
Russ Cox post [Our Software Dependency Problem](https://research.swtch.com/deps),
adding dependencies got so easy in the NodeJS community that lead
to abuse and the lack of thought on the consequences and implications
of depending on a piece of code maintained by a third party.

# Memory Sharing vs Message Passing

The research does try to measure how much message passing primitives are used
when compared to shared memory primitives. The article is not clear
enough on how exactly this have been measured, but if it was by how much
mutexes and how much channels are created the comparison may not be
fair. In the end Memory Sharing is used more than message passing, something
like 70% of the primitives are for memory sharing, only the rest is message
passing related primitives.

The problem with the comparison is that
when channels are used they usually encapsulate more coarse grained
logic, like developing concurrent services, while mutexes are used to
serialize access to finer grained data, hence usually you have more mutexes
than channels if you try to solve the same problem, unless you create a lot
of contention. Of course my opnion on this matter is also produced by
empirical evidence and has the same drawbacks of the entire research =).

# Different ways to do the same thing

One thing that does make some sense is the fact that Go has a lot of
different concurrency primitives, allowing to a lot of alternative
designs and even hybrid designs. Some of the bugs demonstrated on the
article are a mixture of channels and mutexes for example.

Since the space for design and combination is bigger, so does seem
to be the space for bugs. That got me thinking on how things happen
in a concurrent language that is more restrictive, like Erlang,
obligating the user to always use message passing. Do the bugs
transfer one to one and you have the same amount of bugs
but with different primitives, like new bugs with message
passing, or do you get fewer bugs ?

Probably it won't be like all the bugs that existed with
mutexes will cease to exist, some algorithms are just hard,
but I have an intuition that you probably will have fewer bugs,
and without sharing, no races at all (at least for memory), a whole
class of bugs disappears, perhaps new one arises. But having both
ideas on the same language gives you freedom to produce both kinds of
bugs and even combinations of them.

This gets me thinking about how the art of design kinda is the
art of constraining, constraining the right things, it is from
constraints that computing models emerges, like not being able
to "go to" arbitrary memory locations, static strong type systems, etc.

# Conclusion

TODO: very interesting stuff and give good ideas like static analyzers, but
some of the claims are too bold given the interesting but small sample set.
