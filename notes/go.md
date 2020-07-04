<!-- mdtocstart -->

# Table of Contents

- [Go](#go)
    - [Tips](#tips)
        - [Interfaces](#interfaces)
        - [Concurrency](#concurrency)
            - [Orchestrate or serialize](#orchestrate-or-serialize)
            - [Limiting concurrency](#limiting-concurrency)
        - [Reflection](#reflection)
    - [Odd Stuff](#odd-stuff)
        - [JSON Unmarshaling is case insensitive](#json-unmarshaling-is-case-insensitive)
        - [Slices not comparable](#slices-not-comparable)
        - [Method Sets](#method-sets)
        - [Referencing interfaces and maps](#referencing-interfaces-and-maps)
        - [Equality](#equality)
        - [Channel Behaviour](#channel-behaviour)
            - [Nil channels](#nil-channels)
                - [Closed channels](#closed-channels)
                - [Select behaviour](#select-behaviour)
                - [More info about it](#more-info-about-it)
        - [Strings](#strings)
        - [Variadic Args, Interfaces and Covariance](#variadic-args-interfaces-and-covariance)
        - [Enums](#enums)

<!-- mdtocend -->

# Go

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
are also possible using channels, at least I was able to do that,
but perhaps its just that I'm good at writing bug software.

The truth can be found on another Rob Pike phrase:

```
Don't communicate by sharing memory, share memory by communicating
```

This exacerbates an important truth in Go, both models have shared
memory, the only difference is that channels make communication the
first class citizen, while mutexes don't, mutexes make serialization
of access explicit, but it is not clear if communication is going on.

So a good rule of thumb is, do you want to communicate two or more
concurrent processes or you just want to serialize access to some data ?

Answering this will help you find the right tool to the job, channels
won't be always the right tool, in some cases they can even make
things worse.

With channels, your approach is to only allow one goroutine to have access
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

So you can't compare slices or use them as keys on maps,
but you can do this with arrays.


### Method Sets

There is some documentation on the concept [here](https://github.com/golang/go/wiki/MethodSets).
And you can find a lot of references to the concept in the [spec](https://golang.org/ref/spec#Method_sets).

Perhaps this is something that is commonplace in languages and I'm just ignorant
to it, for me it was something new that did not made a whole lot of
sense until now. Perhaps this is unique to Go because it is one
of the few languages that are considerably high level, have a GC,
and yet also have pointers and object values instead of just
references (Python) or always copying (Clojure/Erlang).

Definition quoting from the spec:

```
A type may have a method set associated with it. The method set
of an interface type is its interface.

The method set of any other type T consists of all methods declared
with receiver type T. The method set of the corresponding pointer type
*T is the set of all methods declared with receiver *T or T (that is,
it also contains the method set of T).

Further rules apply to structs containing embedded fields, as described
in the section on struct types. Any other type has an empty method set.
In a method set, each method must have a unique non-blank method name.

The method set of a type determines the interfaces that the type implements
and the methods that can be called using a receiver of that type.
```

The concept (that can be a little confusing at start) is that when
you define a type T you are actually defining two types, T and \*T.

For example, given this code:

```go
type MyType struct{}

func (t *MyType) PointerMethod() {
}

func (t MyType) ValueMethod() {
}
```

The method set of **MyType** is:

```
ValueMethod
```

The method set of **\*MyType** is:

```
ValueMethod
PointerMethod
```

So when you call **PointerMethod** using a value object:

```go
value := MyType{}
value.PointerMethod()
```

This can't actually work, **value** is a variable of type **MyType**
and its method set does not have **PointerMethod**. But in the end
this does work, why ?

Because the Go compiler will transform the code into this:

```go
value := MyType{}
(&value).PointerMethod()
```

Go's Method sets is conceptually odd to me. The pointer type has all methods
of the value type, but if the pointer is nil, calling a method that has
a value as its receiver will cause a nil pointer dereference error.

So conceptually the pointer type has all the methods, but can't call
all the methods safely when it is nil. The pointer receiver methods
are safe to call, but the value receiver ones are not. Lets make this
clear with an example:

```go
package main

import "fmt"

type MyType struct{}

func (t *MyType) PointerMethod() {
	fmt.Println("PointerMethod")
}

func (t MyType) ValueMethod() {
	fmt.Println("ValueMethod")
}

func main() {
	var nilpointer *MyType
	
	nilpointer.PointerMethod()
	nilpointer.ValueMethod()
}
```

Running this code will give you this result:

```
PointerMethod
panic: runtime error: invalid memory address or nil pointer dereference
[signal SIGSEGV: segmentation violation code=0xffffffff addr=0x0 pc=0xd7a04]

goroutine 1 [running]:
main.main()
	/tmp/sandbox644642469/prog.go:19 +0x84
```

A variable of **\*MyType** can fail catastrophically when calling a value
receiver method.

On the other a variable of type **MyType** can ALWAYS satisfy a method call,
independent if it is a value or a pointer receiver, since the value is always
initialized with something it can always be referenced:

```go
package main

import "fmt"

type MyType struct{}

func (t *MyType) PointerMethod() {
	fmt.Println("PointerMethod")
}

func (t MyType) ValueMethod() {
	fmt.Println("ValueMethod")
}

func main() {
	var val MyType
	val.PointerMethod()
	val.ValueMethod()
}
```

So it is more intuitive to me that the type **MyType** should have
as method set all methods, since it can call value and pointer receiver
methods safely , and the type **\*MyType** should have only pointers receivers
methods since it is not safe to call a method with a value
receiver from a pointer type.

You may be thinking...why care ? All this method set discussion
seems useless. When calling methods both value and pointers
will work on any method defined for a given type. If it is a value calling a
pointer receiver method it will be referenced. If it is a pointer it calling
a value receiver method it will be dereferenced.

The problem arises when you are working with interfaces. The concept
of method sets is enforced when validating if you implement, or not,
a given interface. Extending the previous example:

```go
package main

import "fmt"

type MyType struct{
	a string;
	b int;
}

type MyIface interface {
	PointerMethod()
	ValueMethod()
}

func (t *MyType) PointerMethod() {
	fmt.Println("PointerMethod")
}

func (t MyType) ValueMethod() {
	fmt.Println("ValueMethod")
}

func callIface(i MyIface) {
	i.PointerMethod()
	i.ValueMethod()
}

func main() {
	var nilpointer *MyType
	
	callIface(nilpointer)
}
```

Will build and run, producing:

```
PointerMethod
panic: value method main.MyType.ValueMethod called using nil *MyType pointer

goroutine 1 [running]:
main.(*MyType).ValueMethod(0x0, 0x40c120)
	<autogenerated>:1 +0xa0
main.callIface(0x1344c0, 0x0)
	/tmp/sandbox509132386/prog.go:25 +0x60
main.main()
	/tmp/sandbox509132386/prog.go:31 +0x40
```

It is interesting that the panic when using interfaces is more
explicit on what went wrong, the interface indirection allows
Go runtime to validate this better, but as mentioned before
the variable with the type \*MyType satisfies the interface
but it can't call all the methods because it is nil.

A variable with type MyType would be able to call it, but trying
to build the code:

```go
./prog.go:31:11: cannot use value (type MyType) as type MyIface in argument to callIface:
	MyType does not implement MyIface (PointerMethod method has pointer receiver)
```

And this was the error that led me to the method set thing, which until
today I still find odd, because it would be safe to call **PointerMethod** in
this case. I have a suspicion that the reason for this is a implementation
detail, when creating an interface from a value a copy of the value will
be created inside the interface instance.

Allowing a pointer receiver method in this case would mean that the pointer
passed on the method would be from internal memory allocated by the interface
implementation, allowing pointers to this memory to be accessed by the user code
probably complicates garbage collection, so this is prohibited.

This is just a theory for now, I never confirmed it so far, but it makes sense
specially when it is allied to limitations referencing interfaces and maps.
If this is true, the whole method sets thing is kinda lame because it is
a very high level idea that exists just because of some internal
details and optimization on how memory is managed, it does not make
sense conceptually, at least not for me and not right now.


### Referencing interfaces and maps

Why can't you reference a interface like:

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
and harder to do garbage collection (memory that is internal to the
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

### Strings

String behavior in Go can be quite inconsistent and confusing,
not in a buggy way, but in a "what will len return in a string ?" way.
This one is explained with code =):

```go
package main

import "fmt"

func main() {

	fmt.Println(`
The idea of this code is to show that string behavior
in Go is not consistent and that implementation details leaks.
strings are actually byte slices and they behave as one to almost
all operations, but not all, hence inconsistent.
`)
	samplestr := "canção"
	fmt.Printf("string[%s]: len[%d] type[%[1]T]\n", samplestr, len(samplestr))

	fmt.Println(`
The length of a string behaves as a byte slice, giving a size in bytes,
not a size in valid characters (or runes).
Iterating the string with a numeric for will also get you byte slice behavior:
`)
	bytesCount := 0
	for i := 0; i < len(samplestr); i++ {
		b := samplestr[i]
		fmt.Printf("index[%d] value[%v] type[%[2]T]\n", i, b)
		bytesCount += 1
	}
	fmt.Printf("iterations through len: %d\n", bytesCount)

	fmt.Println(`
But iterating the string with range will get you rune slice behavior.
This is so inconsistent that you will see something very odd happening, the index
on the for loop will increment by two on some iterations.
`)
	runesCount := 0
	for i, r := range samplestr {
		fmt.Printf("index[%d] value[%v] type[%[2]T]\n", i, r)
		runesCount += 1
	}
	fmt.Printf("iterations through range: %d\n", runesCount)

	fmt.Println(`
Summing up: len and index operations behave as byte slice
but range iteration behaves as a rune slice.

Quite inconsistent and showcases that it is not safe to use
len or index a string (it will only work for ASCII).

A string is a set of 'something' that has different
cardinality and values depending on how you iterate or access it.
`)
}
```

### Variadic Args, Interfaces and Covariance

This has caused me some problems in the past and I can't understand
why it should not work. If you accept a parameter as variadic args
with the type interface{} you can't "spread" an slice as a parameter
when calling the function, even though anything implements the empty
interface. For example, this works:

```go
package main

import "fmt"

func vargs(a ...int) {
	fmt.Println(a)
}

func main() {
	vargs(1, 2, 3)
	vargs([]int{1, 2, 3}...)
}
```

This also works:

```go
package main

import "fmt"

func vargs(a ...interface{}) {
	fmt.Println(a)
}

func main() {
	vargs(1, 2, 3)
}
```

But this doesn't:

```go
package main

import "fmt"

func vargs(a ...interface{}) {
	fmt.Println(a)
}

func main() {
	vargs([]int{1, 2, 3}...)
}
```

Resulting in the error:

```
./test.go:11:13: cannot use []int literal (type []int) as type []interface {} in argument to vargs
```

This seems related to the fact that when you pass multiple individual
arguments they are converted into an slice of those arguments, and the slice
will have the type required by the called function, in this case []interface{}.

But when you spread (the ... operator) an slice to pass as a variadic argument
it will just pass the slice as it is to the function (pretty efficient) but
then causes the error mentioned above.

This seems related to the concept of
[covariance](https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science))
and the fact that in the case of slices Go does not support it.
Even when type1 matches type2 an []type1 will not match []type2.
Which does feel kinda off because it seems perfect valid.
I guess this would make the compiler and the runtime
more complex, hence why it is disallowed.

In the end this kind of idiom is invalid by a mixture of a design decision
on variadic parameters + design decision on slice covariance.

When designing functions that are generic, using variadic interface{}
parameters, this can be annoying.

### Enums

There is also the lack of enums, which to be honest annoys me terribly
in Go (by far the most). It is quite poor to just use constants because
you can't get compile time safety of the value is inside the set of
allowed values, I always end up having to check that in runtime :-(.
