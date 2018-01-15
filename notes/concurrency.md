# Fan-out / Fan-in

## What ?

Fan out refers to the action of starting N multiple
workers to do a part of a job for you concurrently.

The fan in part refers to the fact that usually you want to
wait for all workers to finish and aggregate the results
so you can use them.

It remembers how map/reduce works. You distribute the
problem to scale better but in the end you want
one set of results.

The basic problem is reading results from N concurrent
(possible parallel) workers.

Here I focus on exploring local concurrency/parallelism
and how a language may support that.

Since right now the best language for that that I know
is Go I will use Go (1.X) syntax for examples.

## Why ?

Usually this is a performance matter. Some problems are
so computing expensive that requires this and others
are naturally expressible this way.

An example is captcha breaking, after you segment the
digits (audio or image) the action of recognizing is
independent between digits. If you have 12 digits you
could classify the 12 digits concurrently. But you need
an easy way to wait all of them to finish and build the
final solution to the challenge.

This example will be used as a basis to discuss possible
solutions to this.

## How

So how can we implement the action of starting multiple
workers and wait for all of them to finish ?

Depending on the language this will vary, and even considering
Go there is also a plethora of possible solution that one can
imagine.

Here we explore some ideas that are possible right now
and some that are inspired on other languages that also
seems nice as a language feature.

Since the discussion involves Go it will be heavily biased
towards CSP (communicating sequential processes).

### Multiplexing Channels

The most common way to wait for any of N channels to have
something to read from is using something like
[Go's select](https://golang.org/ref/spec#Select_statements).

One obvious limitation is the lack of support for a runtime
configurable number of channels. In a select statement you must
statically define how much channels are involved on
the communication at compile time.

Even with a fixed amount of workers, like a captcha with 6 digits,
you still need some way to check if the 6 workers are already finished.

If you choose using just one channel to be shared among all workers
you will need some other synchronization/communication entity
to communicate when each worker is done (traditionally a
[wait group](https://golang.org/pkg/sync/#WaitGroup) ).

For me it is cognitively heavier to handle 2 different communication
entities, it is even worse that they are of a different nature
(the design lacks uniformity). Not that I'm against using
synchronization  instead of communication, but on this specific case
of N workers producing results I don't feel it is a uniform simple solution.

The idea is to get the concurrency model of 1 (receiver) <- 1 (worker):

```go
var solutions chan solution = captchaSolver()

for solution := range solutions {
}
```

And expand it to N workers. Go already provides a way to communicate that some
communication has ended (closing the channel). This pattern is so useful
that even iteration on a channel until it is closed is provided by
the language. The idea is to leverage this to solve the fan-in problem.

Trying to reach for this one solution is to use Go's dynamic select
features (using reflection) to build a generic channel multiplexer
(you can implement one each time you need too). The usage would be:

```go
var solutions []chan solution = make([]chan solution)

solutions = append(solutions, captchaSolver(1))
solutions = append(solutions, captchaSolver(2))
solutions = append(solutions, captchaSolver(3))
solutions = append(solutions, captchaSolver(4))
solutions = append(solutions, captchaSolver(5))
solutions = append(solutions, captchaSolver(6))

var muxed chan solution = make(chan solution)

err := muxer.Do(muxed, solutions...)
//handle error, usually runtime type checking =/

for solution := range muxed {
}
```

The main two advantages of this approach is that the captchaSolver
function works exactly the same way it worked on the 1 -> 1 case
(leveraging Go's channel behavior of closing to communicate end of communication).

There is no need to change the function to use a wait group (or other mechanism)
or to create a wrapper function that does that.

The other advantage is uniformity with how we read all responses,
just as it was with the 1 <- 1 case we just need to iterate all the
responses of the channel and when the loop exits we are done, everything
is closed and done.

The major drawback is Go related. Since there is no generic programming
(with parametric types, not interfaces) there is no way to implement
a generic muxer without using reflection. This makes the API a little
awkward to use and introduces the penalty of using reflection. There is
also the cost of an extra goroutine that does the multiplexing for you.

An example of how to implement this idea in Go can be seen
[here](https://github.com/madlambda/spells/tree/master/muxer).

### Language Supported Multiplexing

This one is inspired on the
[Limbo Language](http://doc.cat-v.org/inferno/4th_edition/limbo_language/limbo).

Quoting the specs (9.8. alt statement):

```
 As mentioned in the specification of the channel receive operator <-
 in §8.2.8, that operator can take an array of channels as an argument.
 
 This notation serves as a kind of simplified alt in which all the
 channels have the same type and are treated similarly.
 
 In this variant, the value of the communication expression is a tuple
 containing the index of the channel over which a communication was received
 and the value received. For example, in

    a: array [2] of chan of string;

    a[0] = chan of string;

    a[1] = chan of string;

    . . .

    (i, s) := <- a;

    # s has now has the string from channel a[i]

the <- operator waits until at least one of the members of a is ready,
selects one of them at random, and returns the index and the
transmitted string as a tuple. 
```

The **alt** statement behaves a lot like Go's **select**, but in Limbo
an array of channels has special behavior, just as single channels do.

As discussed before, a static construction like select/alt will not
be able to handle a runtime configurable number of workers and for
a fixed one it can get pretty clumsy to work with.

But this solution provided by Limbo gives a pretty nice way
to express the fan-in without reflection clumsiness.

Lets imagine that Go provides the same thing implemented by
the compiler, we could have this:

```go
var solutions []chan solution = make([]chan solution)

solutions = append(solutions, captchaSolver(1))
solutions = append(solutions, captchaSolver(2))
solutions = append(solutions, captchaSolver(3))
solutions = append(solutions, captchaSolver(4))
solutions = append(solutions, captchaSolver(5))
solutions = append(solutions, captchaSolver(6))

for solution := range solutions {
}
```

The drawback is that arrays of channels behaves differently
of other arrays, it can be misleading.

Also to actually iterate the array of channels you would
need to use a numeric for (tradeoffs, make one case simpler
and the other one harder).

To know if it is closed and which channel:

```go
var solutions []chan solution = make([]chan solution)

solutions = append(solutions, captchaSolver(1))
solutions = append(solutions, captchaSolver(6))

s, ok, i := <- solutions
// Ignoring index
s, ok := <- solutions
// Ignoring if it is closed
s := <- solutions
```
