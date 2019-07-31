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


# Methodology

Concurrency problems are categorized in cause and effect. There are two causes:

* Shared Memory
* Message Passing

And two effects:

* Blocking
* Non Blocking

The effects are the behavior that the bug presents, being blocked forever
(deadlocks are a subset of this) or not being blocked but causing some
other problem (usually races).

The total of bugs analyzed in the article is 171. It is dwarfed by the amount
of Go code that exists in the world in different organizations, but it is better
than nothing at all.

# To be concurrent or to not be concurrent

My personal experience with concurrency in general (so far at least)
is to avoid it if I can. This may seem sad, but only because we as engineers
usually like complex things, taming concurrency is more exciting than
simple sequential code, but one of the most remarkable points of the
article to me is that it corroborates my life experience that concurrency
is hard, even in Go, even with channels, and sometimes specially with
channels. They don't go as far as saying to avoid concurrency, but I do,
in a lot of cases it is unavoidable, like in an HTTP server accepting
concurrent requests, but you can avoid creating 10 extra goroutines for
each of these concurrent requests just because you can and it is "easy".

CSP (communicating sequential process) is a good model and provides good tools,
these tools may make it easier to understand concurrent algorithms,
but easier is not the same as easy, sequential is still easier
than concurrent. You could argue that perhaps CSP is not good enough,
or perhaps our brains are not good enough, most likely both are true,
but in the end I understand message passing better than shared memory,
it is more explicit, but it is not without its perils and hardships.

I remember vividly one presentation on a Go Meetup where the guy talked about
how awesome channels were and how you would have no races or deadlocks. Both
things are false in Go, channels make some things easier, not easy, and it
still is VERY easy to deadlock, cause races or just block code in general.

One of the first findings of the research is that the majority of
blocking bugs, are caused by code like this:

```go
go func() {
    data := <-somechannel 
    doSomething(data)
}()
```

Which assumes that messages are always going to be sent on a channel,
but if it is not just hangs forever (a go routine leak). The same also
happens for goroutines that write something to a channel, if no one reads
it you are screwed. This is so pervasive on all code bases that in the
end message passing loses to shared memory on blocking bugs, 58% of all
blocking bugs are cause by message passing errors, 42% are caused by shared
memory errors.

Making something easier has the tradeoff of abusing, this reminds me of
Russ Cox post [Our Software Dependency Problem](https://research.swtch.com/deps),
adding dependencies got so easy in the NodeJS community that it lead
to abuse and the lack of thought on the consequences and implications
of depending on a piece of code maintained by a third party.

Every time you create a goroutine you need to think how this goroutine
ends, in all scenarios, including failure ones, and this is to ALL goroutines
you start, so just because typing "go" is easy it does not mean that you
should, because writing correct concurrent code is not that easy,
specially for complex algorithms. If sequential is fast enough,
stick with sequential, concurrency in Go is not a free lunch,
unless you think you are better than the developers on the
aforementioned open source projects.


# Usage of Memory Sharing vs Message Passing

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
of contention (one mutex around a lot of different data).

Of course my opinion on this matter is also produced by
empirical evidence and has the same drawbacks of the entire research =).


# Different ways to do the same thing

One thing that does make some sense is the fact that Go has a lot of
different concurrency primitives, allowing to a lot of alternative
designs and even hybrid designs. Some of the bugs demonstrated on the
article are a mixture of channels and mutexes for example.

Since the space for design and combination is bigger, so does seem
to be the space for bugs. That got me thinking on how things happen
in a concurrent language that is more restrictive, like Erlang,
obligating the user to always use message passing and having no sharing at all.
Do the bugs transfer one to one and you have the same amount of bugs
but with different primitives, like new bugs with message
passing, or do you get fewer bugs ?

Probably it won't be like all the bugs that existed with
mutexes will cease to exist, some algorithms are just hard,
but I have an intuition that you probably will have fewer bugs,
and without sharing, no races at all (at least for memory), a whole
class of bugs disappears, perhaps new ones arises. But having both
ideas on the same language gives you freedom to produce both kinds of
bugs and even combinations of them.

Also you could argue that people do not explore message passing
enough in Go because they can always rely on shared memory to
also solve their problem when they fail to design something
with message passing, like people not learning functional programming
properly on languages that are hybrid.

This gets me thinking about how the art of design kinda is the
art of constraining, constraining the right things, it is from
constraints that computing models emerges, like not being able
to "go to" arbitrary memory locations, static strong type systems, etc.

# Conclusion

In the end, even tough a lot of the statements of the paper are too bold
for my taste it is a very interesting read and give good ideas
of follow up work like static analyzers and also gives some insight
on the nature of real world concurrency bugs, even if the sampling is small
it is an interesting exercise. For example it is interesting to see
that most shared memory errors happens on anonymous goroutines
accessing closure variables.

Some other interesting articles:

* [Bell Labs and CSP Threads](https://swtch.com/~rsc/thread/)
* [Concurrent Reading and Writing](https://lamport.azurewebsites.net/pubs/rd-wr.pdf)
* [A Static Verification Framework for Message Passing in Go using Behavioural Types](http://mrg.doc.ic.ac.uk/publications/a-static-verification-framework-for-message-passing-in-go-using-behavioural-types/draft.pdf)
* [Ad Hoc Synchronization Considered Harmful](https://pdfs.semanticscholar.org/5de1/66adfe1ee8a19ce430a9d3435b566bb07185.pdf)
