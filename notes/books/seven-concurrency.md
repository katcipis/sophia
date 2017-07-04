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
but you are **dealing** with a lot of stuff (video/audio/multiple processes).

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
all other send calls for that agent (instead of the optmistic
try again loop model of the atom swap)

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
at least agents do not lock forever in a spin since they are not
"lock free" :-).

#### STM

STM stands for Software Transacional Memory, this is the other
concurrency model presented for clojure. I won't get in details
because on this part I just gave up on Clojure for concurrency
(or for anything).

### Actor Model

For the Actor model the book uses Elixir as an example.

Elixir concurrency model inherits the Erlang concurrency model.
Because of that, it actually has a pretty neat concurrency model,
that is actor based.

The main difference that I see from Go's CSP implementation is that
the actor model is based on knowing the process (actor) that you are
sending the message to. There is no channel, each process has a mailbox
and any process can send a message to any other process.

I'm still getting the implications of this kind of shift on how
you exchange messages, but it actually pretty different.

In Go, goroutines do not even have ID's, they are completely anonymous,
there are only channels. It is like in Go's the first class concept is
the channel.

In Elixir (and Erlang) the first class concept is the process (and its PID).
The mailbox (which acts as a buffered channel) is the anonymous one.

To contrast, in CSP (Go) goroutines are anonymous and channels are the
first class concepts, on the Actor model (Erlang/Elixir) the processes
(like goroutines) are the first class concepts and the channels are anonymous.

This concurrency model reminds me more of services communicating on a
network. Elixir even have a registry of processes so you can communicate
with processes by name instead of having to know their PID.
It is like a very low latency, dynamic, DNS for processes.

Messages rendezvous happens like
[Hoare's original CSP paper](http://spinroot.com/courses/summer/Papers/hoare_1978.pdf),
if the message matches the process name and the message schema matches, the
message is delivered (which is odd, actor based messaging seems more like the
original CSP theory than Go's current CSP implementation).

Extending on the model that is already used for distributed systems Erlang also
provides the concept of supervisors, that are processes responsible for keeping
other processes running. It is the same thing that a cluster orchestration tool
does, like Kubernetes, but locally inside the same VM. Also these processes are
lightweight Erlang processes, not operational system processes.

All this mechanism is also distributable, there is a local name registry and a
global one. The global name registry will broadcast the service that you are
publishing on the entire cluster of Erlang processes. Setting up the cluster
is out of the scope of the book, but once the cluster is built, the code to
register services and communicate with them is trivial. Communicating with a
local or remote process is seamless.

Remote objects fails because error handling and granularity must be different
for distributed systems. This is not the case for the actor model which already
delivers an abstraction where messages can be dropped and process may die
without being able to send a valid answer, you have to code timeouts and try
agains, like you would do with a REST HTTP API. This way code can be truly
transparent between being local or remote.

#### Error handling

Why talk about error handling on a book that is about concurrency ?
Well Erlang approach to error handling is very interesting. Until now I was used
to two approaches:

* Explicit error handling, like C/Go
* Implicit error handling with Exceptions

Erlang provides a third one, "just let it crash". The idea is to not protect
your code against most common errors, like invalid parameters, null stuff, etc.

Just let it crash, and a supervisor process will restart the process for you.
The entire actor model sustain this since message passing does not guarantee
answers, it does not even assume them, you can just send messages. If you need
a request/response model you must build this on top of the message passing
mechanism, and if you want resilient code you need to handle timeouts properly.

If a response is not received, you can just try again (if the operation
is idempotent). Assuming that, any process can just crash while handling
a message and the client will already timeout and send it again to a new
process that will have been initiated.

Erlang call the processes responsible for restarting failed ones supervisors.
They remind me systemd and Kubernetes. This way to model errors seem interesting
since it is already how we obtain resilience on distributed systems. It is not
about avoid failures but being able to recover from them gracefully.

Well, who supervises the supervisor ? This is the same problem that you have
with tools like systemd and Kubernetes. A good monitoring system will help you
detect systemic failure, but a good thing on this approach is that you can
keep your error kernel pretty simple, the part that is responsible just for
restarting stuff, from the book:

```
A software system's error kernel is the part that must be correct if the system
is to function correctly. Well written programs make this error kernel
as small and as simple as possible
```

This approach seems intuitively good, since it is well structured on practices
that have been developed on distributed systems for a long time, and it is
not a coincidence, Erlang (Elixir is based on it) was made as a language to
build resilient and distributed programs, that is why it embeds on the
language as first class citizens concepts used on distributed systems.

I came to the conclusion that Erlang (and perhaps Elixir) does deserve
a great deal for attention since the need for concurrency and distribution
of programs increases, they offer great tools to handle that from the ground
up instead of using libraries.

Specially Elixir has a lot of features that makes it very different from Go:

* Actor model concurrency, more like distributed system than Go's channels
* New approach to error handling that is not repetitive (but it is not exceptions)
* Purely functional (no mutable state, everything is copied, no race conditions)
* Concurrent processes CAN'T share memory (again, no races)
* Dynamically typed (brings all the trade offs when compared to a static one)
* Pattern matching
* Pipelining

The most important features are actually from Erlang, but Elixir offers
some new ones that are quite cool like pattern matching. The concurrency model
composed with pure functional features makes Erlang and Elixir very interesting
languages.

In Go's there is the proverb:

```
Don't communicate by sharing, share by communicating
```

The idea is to make communication explicit, and if you want, use it
to share data. In Erlang/Elixir you just can't share data, it is
always copied, so you will have less options but also less space
for hard to debug concurrency bugs. If the copying mechanism is
well implemented it can offer a good performance and provide a
more safe/resilient way to develop concurrent software.

About the message passing model there is an interesting quote
from Alan Kay:

```
I'm sorry that long ago I coined the term "object" for this
topic because it gets many people to focus on the lesser idea.
The big idea is "messaging".
```

In some sense, Erlang do not have objects but it is more object
oriented than languages like Python/Ruby/C++/Java =). Wrapping up
this chapter of the book, more Alan Kay wisdom:

```
The key in making great and growable systems is much more to design
how it's modules communicate rather than what their internal properties
and behaviors should be.
```

### CSP

For CSP Clojure is chosen, together with a library that implements
CSP. The core of the ideas of CSP can be explained perfectly through
functions, and the way that CSP is explained as focusing on the
communication channel instead of the communicating entities
seems correct.

Basically you create a channel with the chan function (imported from a lib):

```
(def ch (chan))
```

Write with the **>!!** macro and read with the **<!!** macro.
There is buffered channels, just like Go.

There is also some odd stuff like channels that drops writes
when full, or circular channels that drops the oldest message
when a write is performed on a full channel.

But thing REALLY start to smell when it starts to explain the
**go** macro, that will create what would be a goroutine
in Go. Here the language and the library falls extremely short.

The author is well succeeded in explaining why a thread pool can
go wrong if you make synchronous calls, since the thread will
be blocked, exhausting your pool. One solution is asynchronous code,
but that can be hard to manage.

The solution is to provide a synchronous flow using an asynchronous
engine that lies inside the runtime. The **go** macro aims to provide
such a mechanism, delivering a synchronous flow but implementing asynchronous
code behind the scenes, so far so good.

Then, explaining the **go** macro this happened:

```
(go (<! ch))
```

And this:

```
(go (>! ch 3))
```

If you are paying attention you will see that these macros
have only one **!** instead of **!!**. Why ? They are the
"parking version" of the read and write macros.

Yes, he calls the idea of relinquishing the control of the CPU
to the scheduler as "parking". The magical parking version macro
only works inside **go** macros, using them outside the **go** macro
will throw a runtime exception
(the parking version only exists inside the **go** macro),
you must use the **<!!** and **>!!**
on this cases.

But using these non parking version inside **go** macros
can cause your code to deadlock if you lock all the underlying threads
of the thread pool.  This is not detected by the runtime,
you will simply lock all threads.

Basically if you are inside the **go** macro you must use
**<!** and **>!**, or you can get a deadlock. Outside the macro,
using the wrong version tosses an exception.

I'm not totally convinced that this could not be better implemented even
if it is a library, but even if there is a reason it is just another
proof that if a language is built from the scratch with concurrency
as a first class concern it will always be able to provide better
solutions than languages that try something similar with libraries.

They implement the idea of cheap concurrent units like Go, copying the
names, but fail miserably in providing a uniform and clean interface
since the entire language and runtime is not supporting the idea.
