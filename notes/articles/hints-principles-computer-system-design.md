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

Now on the side of the bad spacs we have remote objects and how they were
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

And performance, even though it would also include de-facto functionality:

```
De facto specs, in either function or performance, happen when the code has properties that
clients come to depend on even though they are not in the spec. Unless you can change the
clients, you are now stuck
```
