# Plan9 From Outer Space: Namespaces Unleashed

```
What was unexpected was how well the model could be used to solve a
wide variety of problems not usually thought of as file system issues.
```
From [The Styx Architecture for Distributed Systems](http://doc.cat-v.org/inferno/4th_edition/styx).

```
The representation of all resources as file systems coupled with an ASCII
interface has proved more powerful than we had originally imagined.
```
From [The Organization of Networks in Plan 9](http://doc.cat-v.org/plan_9/4th_edition/papers/net/)

## Holy Grail: A Uniform Interface

There is some holy grails on computing, and one of the biggest
(if not the biggest) is a uniform interface to integrate
all the things, at least all the ones we know.

It seems like this is not feasible, it always "depends", but one
of the things that makes life so scalable is the DNA, and for like
on earth, DNA is the uniform interface that makes everything work.

So even being extremely hard to find this, the quest is worth it,
because it will be the only way that we will achieve any true scalability.

In my view Plan9 and almost everything around it seems to revolve
around this quest for a good uniform interface.

This quest can be seen in other places than Plan9, like the
[REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
dissertation. But let's focus on Plan9 and how they tried to
achieve uniformity.

## In the beginning there where files

```
The integration of devices into the hierarchical file system was the best idea in UNIX
```
From [The use of namespaces in Plan9](http://doc.cat-v.org/plan_9/4th_edition/papers/names).

Indeed it was. Plan9 pushes the UNIX idea to the limit proposing that the
integration of everything should be done as a hierarchical file system.

What would be everything ? Well, everything =), like:

* Networking
* Information on running processes
* Any service that you can think of
* Remote files
* Actual local files

Access to all this will be done via files.
That is why namespaces are central to Plan9, every process has its own
namespace that can be empty or inherited from it's parent. All services
that a process can see and integrate with must be present on this
namespace. Making stuff available on your namespace can be achieved via
mounting.

Services are represented through files. Since files (services) can be
local or remote you need a common protocol between local and remote
file servers, this is achieved through the 9P network protocol and
mount devices.

## Mount away

TODO: image to explain mount device. local and remote mounting + 9P

## Multiplication factor

How the simple abstraction multiplies on different dimensions.

Examples:

* Containers (isolation between processes)
* Building environments (PATH vs union mount)
* Resource sharing (networking gateways, debugging)
* Monitoring/Visibility (just monitor 9P messages, iostat for the rescue)

## Conclusion

TODO: Not everything fit the file abstraction =(
