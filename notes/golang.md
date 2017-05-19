# Golang

This is a mix of some stuff that I learned while developing software in Go
and other stuff that I read at the Go Programming Language book.

## Tips

### Interfaces

Interfaces are a means to abstraction. This tip is from the GOPL book, and
it actually applies to abstraction in general.

Basically it advocates to not overuse interfaces, specially because it
makes things more complex and slower.

The two cases where interfaces are useful are:

* There is more than one implementation for something
* You need to decouple two packages

The more subtle one is the decoupling. You don't have to decouple all
the packages of the world from each other. A system like that would be
a dependency injection hell, hard to understand and slower than a simpler
design. So the decision to decouple must make sense, like the package
you are going to depend on is too big, complex, or it is a hard
design choice that you want to isolate the system from.

Sometimes refactoring to interfaces can be hard later, so the
possibility to use multiple implementations on the near future
may be a reason to decouple early.

### Concurrency

#### Orchestrate or serialize

This is a cool Rob Pike quote:

```
channels orquestrate, mutexes serialize
```

It is commonplace these days to demonize mutexes as something
old and error prone. The truth is that both races and deadlocks
are also possible using channels (at least I was able to do that,
but perhaps its just me that am THAT good at breaking stuff).

The truth can be found on another Rob Pike phrase:

```
Don't communicate by sharing memory, share memory by communicating
```

This exacerbates an important truth in Go, both models have shared
memory, the only difference is that channels make communication the
first class citizen, while mutexes don't, mutexes make serialization
of access explicit, but it is not clear if communcation is going on.

So a good rule of thumb is, do you want to communicate two or more
concurrent processes or you just want to serialize access to some data ?

Answering this will help you find the right tool to the job, channels
won't be always the right tool, in some cases they can even make
things worse.

With channels, your approach is to only allow one goroutine has access
to data by message passing. For example, on a pipeline, after you write
a message passing some pointer through a channel you must NOT use that
message anymore, or you will have race conditions, after the message is
sent, it's data does not belong to you anymore (or just create a copy
when possible). There is this concept that once you send the message,
you don't own it anymore. The language won't enforce that, but if you fail
on do this all the sins of the bad mutex based concurrency will fall upon you
even thought you are using channels.

In the end you must ensure only one goroutine handles the data at
some point in time, you can use design to do that or enforce it with
a mutex. Complex pipelines will fit better with channels, but a simple
concurrency safe map will probably be better off with a mutex.

#### Limiting concurrency

Sometimes it is useful to enforce a maximum amount of running goroutines.
A common way to do this is with semaphores. In Go semaphores are trivial
to implement using channels.

For example, creating a semaphore that allows only 50 to be created:

```
        semaphore := make(chan bool, 50)
        for {
                // acquire, value written does not matter
                semaphore <- true
                go func() {
                        defer func() {
                                // release
                                <-semaphore
                        }()
                        // Do stuff
                }()
        }
```

### Reflection

The first law of reflection is "don't use reflection" :-).
This is a strong statement, but when you use reflection you
wave away all compile time safety that Go's gives you and embraces
all the runtime errors that are commonplace on dynamic languages
with the downside that the code will be cumbersome (at least the
dynamic language is clean to express this dynamism).

Also your code will be from one to even two orders of magnitude
slower (someone remembered Python ? :-). Be pretty sure that you
have no other option when you do this.

## Odd Stuff

### JSON Unmarshaling is case insensitive

This is just crap =(

```
To unmarshal JSON into a struct, Unmarshal matches incoming object keys to the
keys used by Marshal (either the struct field name or its tag),
preferring an exact match but also accepting a case-insensitive match.
Unmarshal will only set exported fields of the struct.
```

### Slices not comparable

Arrays are comparable, but slices are not, this is a common cause of confusion
since it seems like a lack of uniformity on the language.

The main two reasons are (related to the indirection on them):

* Slices can be recursive, implementing comparison on this case is rather confusing/complex
* Slices indirection can give odd behaviour when they are used on maps

For example, slices could be effectively equal
(the data inside them are the same) but they would be hashed differently
depending on when you inserted them on the map.

So you can compare slices or use them as keys on maps,
but you can do this with arrays.


### Method Sets

* [Method Sets](https://github.com/golang/go/wiki/MethodSets)

Why I cant reference maps and interfaces ?

Because of that Golang performs a syntatic sugar when you call pointer methods 
on a value object you dont know that on the start.

If you have a method that has a pointer receiver:

    func (self *MyType) Method()

And you call the method using a value object:

    a := MyType{}
    a.Method()

This cant actually work, a is a value object and its method set does
not have **Method**. But this will actually compile and work because Go does:

    (&a).Method()

You have problems when you try to pass the value object as an interface.
It does not have the method on its method set and interfaces cant be referenced.
That is why your build will fail (Go cant do (&obj).Method() if obj is an interface).

Go's Method sets is conceptually odd to me. The pointer type has all methods
of the value type, but if the pointer is nil, calling a method that received
a value on it will cause a nil pointer dereference. So conceptually the pointer
type has the method, but can't satisfy it because it is nil (and method receivers
that are pointers can work just fine with nil pointers).

On the other hand the value can ALWAYS satisfy a method call that have a
pointer as receiver, since the value is always initialized with something it
can always be referenced. So it is more intuitive to me that value objects
can call value and pointer receiver methods, and pointers should be allowed
to call only pointers methods since it is not safe to call a method with
a value receiver from a pointer type.

### Referencing interfaces and maps

This is related to the method sets problem. The entire method sets thing
exists because of the referencing issue. Why can't you reference a
interface like:

```
(&obj).Method()
```

Or a map, like:

```
var a map[string]int = make(map[string]int)
a["a"] = 1
b := &a["a"]
```

Results in:

```
cannot take the address of a["a"]
```

It is definitely not impossible, it would just make thing more complex
and harder to cleanup garbage memory (memory that is internal to the
runtime).

Data that is inserted on a map reside on buckets that are manipulated
as contiguous memory regions, pretty low level and unsafe stuff.

If the map allows you to hold a direct pointer to this bucket area it
loses control from it and can't guarantee that it will be freed when
a larger bucket needs to be allocated to expand the map (full buckets).

Allowing this would complicate the map implementation and open
space to a lot of inefficiencies and memory garbage. Although it seems
to me that it is a choice.

The same goes for interfaces, when you initialize a interface with
a value object, it will copy the object to a internal memory region
of the go runtime. Every time a method that has a value received is
called it will copy the entire object and pass it to the method call.

But if you want to call a method where the receiver is a pointer, now
you have a problem. To pass in a pointer Go would have to give the address
of its internal memory storage for interface instances.

I don't know how it works in detail (yet), but it seems to be analogous
to the map problem, this would probably cause some problems to the
runtime, so the limitation has been imposed and the whole method sets
thing emerge.

The runtime could copy it and pass the address too, but that would defeat
the semantics of a method with a pointer receiver, that guarantees that
side effects on the object will be permanent.

### Equality

As almost all imperative languages, Go has equality operators, like **==**.
Every language has to define what is comparable and what is not. And
Go does not allow you to override operators, or you can use the operator, or
you can't. When you can't you can of course define a function to do that, which
is usually more explicit than just overriding operators.

The only odd thing is that Go arrays are comparable and slices are not.
It is not that odd, since they are not effectively the same thing, they
are pretty different actually, but for people coming from other languages
it can be a little odd that you can't compare slices.

Slices can't be compared for the same reason that maps can't, they can
be recursive, a slice can have itself inside of it as so does a map.
You can still implement equality on these conditions but it is pretty
hard to do it efficiently and correctly, so Go chose to not implement
it at all.

### Channel Behaviour

* [Nil channel blocks](http://dave.cheney.net/2014/03/19/channel-axioms)

Why the hell should it block instead of panicking ?

My problem with channels is the lack of symmetry/uniformity on the behaviour.

After some time and digging I found out that for each different behaviour
there is a reason, based on a practical case, so it started to make
more sense at least.

But for me sometimes is hard to remember the behaviour of channels depending
on the state/context that they are, although the complexity seems
to be necessary.


#### Nil channels

* Read from nil channel blocks
* write to nil channel blocks

This one is pretty symmetric, so its ok, the problem arises with closed channels.
It seems that both kind of channels are invalid (nil and closed ones), but this idea
may not be true, it just an impression that I have.

Although being symmetric and important to select behaviour, to a newcomer it is 
completely absurd that manipulating a nil variable results in valid behaviour instead
of utterly destruction of the universe as you know it. But this can be related to
the languages that I'm used to, not a universal truth.

A major drawback is that a bug, like having a nil variable, will cause a block condition
on your application (happened to me). More details:

https://groups.google.com/forum/#!msg/golang-nuts/QltQ0nd9HvE/VvDhLO07Oq4J

The original behaviour was to panic, but it has been changed to be consistent with the
select behaviour.


##### Closed channels

* Read from closed channel returns zero value
* Write to closed channel panics

This one is pretty confusing. There is no blocking behaviour at all, and no uniformity between
read/write at all (like nil channels).

But after some reading, this behaviour is pretty useful to use a pattern where only one goroutine
writes stuff and N other goroutines are reading from it. When there is no more jobs to be done
closing the channel will "broadcast" nils (meaning, closed) to all readers (since read returns nil).

Care must be taken with closing, since it will panic. This obligates you to model your application to
always have only one goroutine responsible to close channels, which to be honest seems to be a good idea.

This also maps well with Hoare CSP idea, where you block on a read until data is sent to you or
the process that you are waiting to receive data from terminates (the process terminating is the 
channel being closed). So there is a clean way to know that the producer is finished.

This makes it easy to implement a consumer <-> producer pattern, with N consumers for each producer.

##### Select behaviour

The behaviour is well documented here:

https://golang.org/ref/spec#Select_statements

One of the reasons that I took time to understand this is because I started using channels without
having to use select statements (select is extremely useful).

```
If one or more of the communications can proceed, a single one that can proceed is chosen via a uniform pseudo-random selection.
Otherwise, if there is a default case, that case is chosen.
If there is no default case, the "select" statement blocks until at least one of the communications can proceed.
```

When all channels are not ready, default case is invoked. And that is why a nil channel blocks, it will not be ready, so the
default case will be called. This enables an easy way to **disable** cases, just point the channel to nil and that
case wont be selected anymore. And if all the channels are **disabled** the default case can be used to get out
for a eternal blocked loop.

For someone new to Go it is very hard to understand that setting to nil will disable the case (at least for me it was,
but only on the beginning).

##### More info about it

* [why chan nil blocks?](https://groups.google.com/forum/#!topic/golang-nuts/QltQ0nd9HvE)
* [why write to a closed channel will panic?](https://groups.google.com/forum/#!topic/golang-nuts/1wgT_90Q1dk)
