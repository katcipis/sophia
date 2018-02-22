# Limbo

This is inspired mainly on the
[The Limbo Programming Language](http://www.vitanuova.com/inferno/papers/limbo.html) paper.

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
when you instantiate the module. There is no support for global state
on modules. For two modules to share the same instance of a module they
must do it explicitly, one of them must instate the module and pass
it as a parameter.

This is very different from languages that allow initialization on a module
that is executed only on the first time it is imported and them all
code using that module shares this state initialized only once
(Go has the init function for this). Limbo has no support to that, there is
no way to implicitly share data among modules, at least not using import
mechanisms (there may be others). This is very interesting. It makes using
modules a little more cumbersome but adds a lot of safety on how side
effects ripple through your code.

In the end modules seem more like a first class citizen, just like functions,
they are instantiated and passed as parameters as any other kind of value
on the language. It is the only thing that I found remarkable on Limbo.
Not that the rest of the language is bad, it was just not new for me at least.
