# Hints and Principles for Computer System Design

## Specs

There is an abundance of awesome advice on how to specify computer systems,
it would be pointless to just summarize all here (also a great deal of it leans
towards math and I'm math dumb, so would not be able to give insight into it),
but there is a few thingsthat gets extra attention.

On the hallmarks of good specs, comes simplicity and the importance of
good design, with the bonus round of a Dennis Ritchie quote:

```
A spec should be simple, it should be complete enough,and it should admit code that is small
and fast enough. Good specs are hard, because each spec is a small programming language with
its own types and built-in operations, and language design is hard. Also, the spec mustn’t promise
more than the code can deliver—not the best possible code, but the code you can actually write.

In Dennis Ritchie’s words: “In spite of these limitations, the stream I/O system works well. Its aim
was to improve design rather than to add features, in the belief that with proper design, the features
come cheaply. This approach is arduous, but continues to succeed.”
```

With also the great insight that any problem that includes creating language will be
a very hard problem to solve, designing languages is hard work, they are basically
meta-thinking, you need to think about how you think about something.

One cool personal example that he talks about, on how hard it is
to write specs, is this:

```
In the late 1990s Paul England and I at Microsoft invented the mechanisms that
ended up in the Trusted Platform Module (TPM). By 2002 the TPM had been
standardized, and later I wanted to know some details about
how it worked. So I downloaded the specR107 and started to read it, but I got
stuck right away. Even though I actually invented these mechanisms, I couldn’t
understand the language of the standard.
```

Even the guy that dedicates so much effort and attention to specs, still fails
on creating language that he himself can understand later =P.

Now on the side of the bad specs we have remote objects and how they were
specified in the past. I remember reading something about the main reason
being that the error handling and the performance is way too different from a local
object in order for it to be successfully abstracted as a local object. And on this
paper I got extra confirmation for this, on leaky/bad specs Lampson states:

```
Errors or failures in the code mean that the code won’t satisfy the spec, unless the spec gives
it a way to report them. A common example is a synchronous API that makes the code look
local, fast and reliable even though it’s really remote, slow and flaky.
```

I also like how he models performance/reliability as part of the spec, decoupled
from code. I always see this completely unspecified, or done in an ad-hoc manner
on monitoring systems, usually as an after thought. Some notes on this, regarding
overloading (and probably admission control):

```
Similarly, contention or overload may keep the code from meeting the spec
if there’s no way to report these problems or set priorities.
```

He is actually pretty holistic on what emcompases the specification of
your software, and I tend to agree that working at that level produces
a more well thought system:

```
The spec describes all the possible visible behaviors of the system in terms
of the visible abstract state (which includes the arguments and results
of operations that the client invokes).

Sometimes people talk about a “functional spec”, a vague notion based on
the wrong idea that the system has no internal state or actions,
and that there’s nothing to say about its behavior beyond the results
that its actions return.

But reliability, failures, resource consumption, latency and system
configuration can all be visible state that a spec can describe.
```

On some domains some of this aspects are more explicitly part of the
spec, for example take latency, if the brakes of your care don't have
a clear spec of how much latency it may take to calculate something
you may end up dead with breaks that take seconds to respond =P.
So being precise on latency specification is fundamental on this case.

I also found interesting how he includes configuration as visible state
that needs to be specified, that also makes a whole lot of sense, people
tend to treat configuration as a second class citizen, "it is just a configuration
change", and when you see it is a complete meltdown on the entire system
"just because of a configuration".

One example from [Google](https://status.cloud.google.com/incident/cloud-networking/19009):

```
Two normally-benign misconfigurations, and a specific software bug,
combined to initiate the outage
```

As usual, reality is pretty complex and bad shit happens because 
of multiple different things going wrong at once, the perfect storm, but
it was also related to misconfigurations, maybe the idea of "benign"
misconfiguration should not exist, it is also a bug, a software bug,
given a well defined specification.
