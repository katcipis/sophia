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
[here](./notes/plan9.md) and will not be repeated here when Inferno basically replicates
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
the [Limbo](./notes/limbo.md) language:

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
These details of the Limbo language can be seen in further detail [here](./notes/limbo.md).

# Uniformity

This one is going to be hard =P. TODO.