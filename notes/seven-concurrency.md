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

## Concurrency Models

Will only mention the ones worth it, skipping Java/Threads/Mutexes
for example. If it where low level in C, talking about the operational
system support it would been great, but this kind of stuff in Java is
boring/useless as hell.

### Clojure

One of the concurrent "models" on the book is Clojure's approach
using the concept of separating identity from state + atoms.

Identity/state separation is a fancy work for something very like
copy on write. It is not the same implementation, but it depends on
the same idea to be implemented.

Basically the identifier of a variable can point to different
possible states on a timeline, each identifier holds its whole history,
instead of the current state only (as almost all imperative languages).

The idea is cool since it reflects the real world, the time when you are
referencing a identifier changes the state of it. Like someone with the
identifier **uspresident** from last year would see the state Obama,
this year will be Trump, the identifier holds all its previous values
not only the current one.

This related with copy on write because without some clever hack on
how to manage this information the memory usage would be prohibitive.

From https://clojure.org/about/state:

```
There is another way, and that is to separate identity and state
(once again, indirection saves the day in programming).

We need to move away from a notion of state as
"the content of this memory block" to one of
"the value currently associated with this identity".
Thus an identity can be in different states at different times,
but the state itself doesn’t change.

That is, an identity is not a state, an identity has a state.
Exactly one state at any point in time. And that state is a true value, i.e.
it never changes. If an identity appears to change, it is because it becomes
associated with different state values over time. This is the Clojure model.
```

So basically what would be a race condition would not happen, it will
be substituted with stale data, if someone is updating a list while you
iterate on it there will be no race, you will just iterate the list that
is a snapshot of the moment you started iterating.

#### Atoms

One of the facilities to handle concurrency are atoms.
They are based on the function swap that is called to change
the value of the atom safely.

Race conditions are handled by detecting that another function already changed
the atom value and calling the swap function again.

This implementation is called "lockless" but it actually is a spin lock,
it is so a spin lock that it can even generate starvation. If your
swap function is too slow and there is a lot of threads with faster
swap functions being called your swap function will starve and will never
be able to update the value, locking forever (worse than a spin lock).

Besides the stupidity of the forever lock I also fails on see any advantage
on this approach, you are just serializing access to state, on a very odd way,
yay Clojure.

#### Agents

OMG, Clojure only gets better. They have a **agent** concept that
is not an agent at all. It is basically the same thing as a atom,
the only diference is that the swap function is called **send**
and the execution of the send function will be serialized with
all other send calls for that agent.

It happens on a thread pool, or you can choose to create one just
for your send. It also has a pretty odd failure recovery.

Another key difference is that it is asynchronous, after you call
send the agent may not have yet mutated, it will happen someday,
and you can call a wait function for it.

The send function basically receives the agent as a parameter and
will set the return of the function as the new agent value.

This is the most fucked up thing I have ever seen in my life,
this is not an agent, you are not sending a message, you are executing
a function that will mutate the contents of a value. Really hard
to find anything cool/useful on Clojure concurrency model.

Besides being asynchronous, it has no difference from atoms. Well,
at least agents do not lock forever in a spin :-).
