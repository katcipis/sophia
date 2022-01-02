# Limbo

This is inspired mainly on the
[The Limbo Programming Language](http://www.vitanuova.com/inferno/papers/limbo.html) paper
but is has also been augmented with some information from 
[The Inferno Operating System](ftp://ftp.mrynet.com/operatingsystems/inferno/paper01.pdf).

It is more like some highlights of what I found interesting on Limbo.

The language has been used to develop the Inferno OS on bell labs.
It runs on top of a VM and it is statically typed.

# Concurrency

One of the major wins, besides being high level and garbage collected,
is the concurrency model proposed by the language, as a first class
citizen, which on this case is CSP.

It goes to the extreme of providing no primitive synchronization
mechanism, it has only channels to communicate between concurrent executions.

To create new threads there is the **spawn** statement:

```
 The term and expression-list are taken to be a function call.
 Execution of spawn creates an asynchronous, independent thread of control,
 which calls the function in the new thread context.
 
 This function may access the accessible objects in the spawning thread;
 the two threads share a common memory space.
 
 These accessible objects include the data global to the current module
 and reference data passed to the spawned function.
 
 Threads are preemptively scheduled, so that changes to objects used in
 common between threads may occur at any time.
 
 The Limbo language provides no explicit synchronization primitives;
 ```

I don't remember reading anything that explains if the thread is
a green thread or a system thread, but my brain sucks =/.


The concurrency model is awfully a lot like Go, actually Go is a lot
like Limbo, which makes sense since Rob Pike worked directly on Limbo
with Denis Ritchie. The **alt** statement is very similar to the
**select** statement from Go.

On the concurrency side there are some interesting disparities between
Go and Limbo. Go has no easy way to perform multiplexing of N channels,
while on Limbo it has support to read operations on lists of channels,
it automatically multiplexes the entire list on a single read operation.

On the other hand, Go has support to buffered channels while Limbo requires
you to implement buffering by hand. It is an interesting instance of the
tradeoff's usually done on language design.

# Module System

Other remarkable feature is the module system. It is similar to C in the
sense that it is composed by headers + implementation files. This leads
to duplication and these days I'm prone to not liking it. But the most
interesting idea is the division of modules as static/const data and
dynamic/mutable data. With a simple import of a module you can only
access constant data, which is allocated statically and never changes.
Every mutable data (and all functions) of a module can only be accessed
when you instantiate the module. There is no support for global mutable state
on modules. For two different modules to share the same instance of a third
module they must do it explicitly, one of them must instate the module and pass
it as a parameter to ther other.

This is very different from languages that allow initialization on a module
that is executed only on the first time it is imported and them all
code using that module shares this state initialized only once
(Go has the init function for this, on interpreted languages like python
it is everything on the main body of the module).

Limbo has no support to that, there is
no way to implicitly share data among modules, at least not using import
mechanisms (there may be others). This is very interesting. It makes using
modules a little more cumbersome but adds a lot of safety on how side
effects ripple through your code.

In the end modules seem more like a first class citizen, just like functions,
they are instantiated and passed as parameters as any other kind of value
on the language.

One of the main motivators for this design is the constrained environments
where Inferno was supposed to run, this dynamic model of loading modules
enables code to just load the modules that it actually needs and to release
all the resources of the loaded module when it is not needed anymore.

On a more traditional language (almost everyone I know) you load modules
at the main of your program, this happens automatically, and after a module is
loaded it will never be unloaded. This is simpler to use but has the drawbacks
of having implicit shared (possibly mutable) state and loading and keeping in
memory stuff that is never going to be used.

A use case that exacerbates this is CODEC handling. Lets say you are implementing
a video player. There is 3 components to video playing:

* The container of the format (AVI, MP4, etc)
* The video CODEC (h.264, vp8, vp9)
* The audio CODEC (mp3, PCM, ogg)

The possible combinations of these formats are huge. Now imagine that the video player
has to statically load in memory all the CODECS that is supports. Even if this loading
is something very basic just as some constants and functions, or a huge number of formats
and in  a constrined environment it can be a problem. In a language like Limbo, it is
easy to develop a player that will only load the necessary container/video/audio modules,
and only when they are required (reminds me of lazy evaluation).

Of course there is a clear tradeoff in usability and even readability. For someone used
to Go, or example, seeing modules being imported and used in Limbo is pretty awckward.


# Garbage Collector


Limbo garbage collector has two distinguished features that caught my attention.
From [The Inferno Operating System](ftp://ftp.mrynet.com/operatingsystems/inferno/paper01.pdf):

```
Because view was the top- level function in this process,
the process will exit and free all its resources.
All memory, open file descriptors, windows, and other
resources the process holds will be collected as garbage when the return executes.

The Limbo garbage collector uses a hybrid scheme that combines
reference counting with a real-time sweeping algorithm.

Reference counting allows reclamation of memory the instant its last reference disappears;
the sweeping algorithm runs as an idle-time process to reclaim unreferenced circular structures.

The instant-free property means that system resources like file descriptors and 
windows can be tied to the collector for recovery as soon as they are no longer used.

This property allows Inferno to run in smaller memory arenas than those required
for efficient mark-and-sweep algorithms, and it also provides an extra level of programmer convenience.
```

Resuming:

* The GC is hybrid, it will use simple ref count when possible
* The GC collect other resources besides memory, like file descriptors and window handlers

The "process" described on the given note is a limbo process, not a operational
system process (like a goroutine in Go). This is another very interesting aspect of the
language since it will avoid some cases where file descriptors leak (not all of them I think).

If it is guaranteed that your process exit, and it does not share any of its resources, even if
you dont release then manually (with close, for file descriptors) they will be released automatically.
Indeed it is an extra level of convenience for the programmer =).