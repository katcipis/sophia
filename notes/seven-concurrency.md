# Seven Concurrency Models In Seven Weeks

## Concurrency or Parallelism

Sometimes it is hard to separate one from another. One cool
phrase from Rob Pike is that helps with this is:

```
Concurrency is dealing with multiple things at the same time,
parallelism is doing multiple things at the same time
```

The key difference is **dealing** vs **doing**. On a concurrent
system, like a operational system, you are not necessarily doing
things at the same time (think about having only one CPU/core),
but you ARE dealing with a lot of stuff (video/audio/multiple processes).

So you usually use concurrency when you need to deal with lots of
stuff at the same time, providing feedback and partial results
to each one of them, even when you have only one CPU to run,
by definition everyone is competing for that resource.

Concurrency always imply that race to compete for resources, and
usually introduces non-determinism, since the scheduling on the
CPU can't guarantee the same order of execution, parallelism don't
have to be non deterministic.

For example, lets say you always split an array in two and process
them in parallel using some special parallel hardware. There is no
reason at all to not have always the same result, there is no
shared state or concurrency whatsoever involved on doing this.

So, besides being confused, one thing has little to do with another.
Concurrency usually belongs to the problem domain, you need to
deal with multiple things at the same time to satisfy the problem
requirements. Parallelism is from the solution domain, you just
want to solve the problem faster, it is an optimization, but it
is not related to the problem (you could argue that solving
something faster is part of the problem if the requirement is
"finish in 30 seconds").

## Parallel hardware and concurrency models

It is not explicit on the book but I found an underlying
pattern on how parallel hardware works and concurrency models
that are implemented on software.

Multiprocessor architecture can be implemented with shared memory
(x86/x64) or distributed memory. On the shared memory model, each
processor/core behaves as a thread, accessing the same memory
pool. To avoid races and enable coherent caching a lot of locks
and mechanisms are required.

On a distributed memory model, each processor has its own memory,
no processor can access it besides it's owner. If a processor need
something that resides on the memory of another processor it will
have to send a message to that processor (via a specialized bus or
network). This is analogous to a design where each concurrent
execution unit (defined in software, as a thread or goroutine in Go for example)
has exclusive access to its own memory and sharing
only happens through explicit communication.

In the end, you communicate by sharing memory or you share
memory by communicating. Of course you can implement a copy based
message passing where nothing at all is shared, there is only
communication, which is safer.
