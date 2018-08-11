# The Inferno Operating System

These notes are based on a lot of different articles, like:

* [The Styx Architecture for Distributed Systems](http://doc.cat-v.org/inferno/4th_edition/styx)
* [The Inferno Operating System](ftp://ftp.mrynet.com/operatingsystems/inferno/paper01.pdf)

# Why ?

Why Inferno has been developed ? From what I understand there are two main motivators:

* The prevalence of distributed systems, so the need to enhance network programming
* The distributes systems have a wide variety of services/content/protocols
* The need for something easy to embed on a constrained system

From [The Inferno Operating System](ftp://ftp.mrynet.com/operatingsystems/inferno/paper01.pdf):

```
The InfernoTM operating system facilitates the creation and support
of distributed services in the new and emerging world of network environments,
such as those typ- ified by CATV and direct satellite broadcasting systems,
as well as the Internet.

In addition, as the entertainment, telecommunications, and computing
industries converge and interconnect, different types of data networks are arising,
each one as potentially useful and profitable as the telephone network.
However, unlike the telephone system, which started with standard terminals and signaling,
these new networks are developing in a world of diverse terminals,
network hardware, and protocols.

Inferno is designed so that it can insulate the diverse providers of content
and services from the equally varied transport and presentation platforms.
```

From what I understand, Inferno has been made for what we call today **IOT**(Internet Of Things).
The vision that the future would converge to this was already present, and a programming
environment that tammed the complexity of such different services and hardware
where necessary. The main concept of Inferno is this necessity for uniformity, from which
it borrows heavily from [Plan9](https://en.wikipedia.org/wiki/Plan_9_from_Bell_Labs).

A lot of the advantages of how uniformity is approached on Plan9 are already listed
[here](./plan9.md) and will not be repeated here when Inferno basically replicates
the same behavior. But there is a lot of new things that are exclusive to Inferno, and
also some of the Plan9 ideas will be extended and better explained here too on the
context of Inferno (the Inferno papers explained them better).


# Embedding

Inferno has been made to fit on really constrained environments. This can be seen by
the lack of need for a memory management unit on the processor:

```
Inferno runs useful applications as stand-alone programs on machines with
as little as 1 MB of memory, and it does not require memory-mapping hardware.
```

Even without memory protection from the CPU you can program safely on Inferno thanks to
the [Limbo](./limbo.md) language:

```
Limbo is fully type-checked at both compile and run time.
For example, pointers—besides being more restricted than in C—are checked before being 
dereferenced, and the type-consistency of a dynamically loaded module
is checked when it is loaded.
Limbo programs run safely on a machine without memory- protection hardware.
```

Even tough Limbo is a higher level language comparing to C, it has a lot of features
that are focused on maintaining a small footprint of memory usage, which makes it
suitable to constrained enviroments as the ones found in embedded systems. The garbage
collector and module systems of the language also aids to make its footprint smaller.
These details of the Limbo language can be seen in further detail [here](./limbo.md).

# Dis Virtual Machine

One of the biggest differences between Inferno and Plan9 is the choice to build
a virtual machine and a higher level laanguage on top of that to develop applications
for the operational system. You can still develop in C but there is a clear trend
towards developing everything in a higher level language.

There are separated notes just for the Limbo language, which is a very interesting language
indeed. Here I try to express the main interesting things from the virtual machine where
Limbo runs on top.

There notes are mainly based on:

* [Dis Virtual Machine specs](http://www.vitanuova.com/inferno/papers/dis.html)
* [The design of the Inferno virtual machine](http://doc.cat-v.org/inferno/4th_edition/dis_VM_design)

On the design choices two things makes the Dis virtual machine pretty different from
the JVM:

* It is optimized to use as little memory as possible
* It is a memory machine instead of a stack machine

The using little memory thing is made clear on the design of the garbage collector
that uses simple reference counting as far as possible instead of other more
complicated algorithms (it uses a mark and sweep for possible cycles, a well
know limitation of reference counting).

The tradeoffs between a memory machine and a stack one are less know to me
because of ignorance on the subject. Basically a stack machine looks like this:

```
push  a      # LS

push  b      # LS

add          # LLS

store c      # LS
```

Which reminds me a lot of the Lua API, which is also stack based.
This on a memory transfer machine would look like this:

```
add   a,b,c  # LLS
```

There are tradeoffs between the two designs, but they argument that
designing a virtual machine with a instruction set similar to the underlying
processor where it will run makes a lot of things easier, and the idea
of implementing stack processors seem to have been rejected at Bell Labs:

```
One might argue that a stack-based processor design would mitigate the difficulties,
but our experience with the implementation of a stack machine in the AT&T Crisp microprocessor
leads us to believe that stack architectures are inherently slower than register-based machines.

Their design lengthens the critical path by replacing simple registers with a complex stack cache mechanism.

In other words, it is a better idea to match the design of the VM to the processor than the other way around.
```

The design leads to something that is easier to implement a processor for, but they argue that
this is less interesting than it sounds like, anyway since it uses much less memory
a dedicated processor to Dis seems more viable than the ones for the JVM that
requires a lot of memory to operate, and memory on the level of the processor is
pretty expensive (time or cost itself).

Reading the spec is an interesting exercise, the only instruction set that I know is
MIPS and it is an actual processor, it is much more low level with the basics
of a processor:

* Control Unit
* ALU
* FPU (optional)

The Dis instruction set is remarkably high level, it has instructions to do
things like:

* Spawning new threads/processes
* Loading modules
* Calling functions on modules
* Object allocation with type descriptors
* Manipulating strings
* Manipulating arrays (head/tail/and others)
* Primitives for comunicating concurrent processes (CSP)

It is very interesting to see that, there are some instructions that are
said to be there to help to implement new languages for the VM, but to
be honest that does not seem like a very good idea since the VM seems
to be pretty specific for the Limbo language (not that this is a bad idea).
A language that fully uses the features of the VM would be very much like Limbo.

The Dis binary itself seems to be pretty simple and clean. One interesting flag
is the **runtime_flag**:

```

The runtime_flag is a bit mask that defines various execution options for a Dis module.

The flags currently defined are:

MUSTCOMPILE	= 1<<0
DONTCOMPILE	= 1<<1
SHAREMP		= 1<<2

The MUSTCOMPILE flag indicates that a load instruction should draw an error if the implementation
is unable to compile the module into native instructions using a just-in-time compiler. 

The DONTCOMPILE flag indicates that the module should not be compiled into native instructions,
even though it is the default for the runtime environment.

This flag may be set to allow debugging or to save memory. 

The SHAREMP flag indicates that each instance of the module should use the same module data
for all instances of the module.

There is no implicit synchronization between threads using the shared data. 
```

Again it can be seen a focus on performance by the use of a JIT compiler and the SHAREMP
feature seems to be present just to allow different languages to implemented on top
of the VM since AFAIK Limbo always instantiates modules, they never can share data.
Implementing Go packages on top of the VM would have to use SHAREMP for example.