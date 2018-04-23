# Intel Clear Containers

This notes are based on the whitepaper Intel® Clear Containers: A breakthrough combination of speed and workload isolation.

Sadly the white paper right now can't be found on the internet =/, broken links.

One of the first sentences on the paper already sounds off:

```
Virtualization has enabled the decoupling of software workloads from
the hardware they execute on
```

What ? AFAIK the operational system does this. The operational system
is the virtual machine that languages are targeted at, not some processor
feature. This felt extremelly off if not plain wrong. Not a good start for the paper.

Before I go on, I'm biased, even before reading the paper I was really sad
with the idea that operational system have made security and isolation so
hard to implement that implementing them on hardware seems to be a good idea.

To think about using hardware features for virtualization just to promote isolation
between processes seems like a failure of imagination on how to implement these
things better on the operational system level, it is a terrible defeat for a
operational system (contrast the intel clear container ideas with Plan9 and Inferno
for example).

When you think about security, it seems to me that the only thing that the hardware
needs to do is to provide isolation primitives, between processes. And even that is
only necessary on the hardware level because implementing them on software would
be too slow. So it is an optmization, not the case where conceptually it is safer
on hardware. By the way, this leads to the second point...implementing logic on hardware
is not safer. It seems safer because you can't change it without phisically changing
the hardware, but this is also what makes it unsafe..YOU CANT CHANGE IT (think about
meltdown and specter). Since hardware is HARD (it wont change) it seems that complex
logic should reside on a layer where updates are easy to do, like software. Hardware
should do the minimum possible to enable software to implement isolation properly.

Another thing that bugs me deeply is that virtualization != isolation. Virtualization implies
isolation, but isolation does not imply virtualization. Actually as far as I know the idea
of hardware virtualization is just to enable different operational systems to think that they
own the hardware when they actually don't. It is the CPU delivering to operational systems
what the operational systems themselvs deliver to applications, the illusion that they own
everything. This has nothing to do with the problem of running just one operational system
and guaranteeing that processes being managed by the operational system are isolated from
each other. So in the end..the whole idea seems pretty stupid and not a breakthrough at all.
It seems interesting just because you need to do some cool hacks to fool virtualization
on thnking that it is loading another kernel when it is not and because cgroups and namespaces
on linux is so fucked up that virtualization enables you to forget how crappy isolation
is on linux.

A quote from the paper:

```
vms are decidely superior for security
```

Why ? Honestly it may be true... but how we got on a point
where hardware virtualization is the safest choice ? Again it seems like
a defeat of operational systems on implement these things properly.
It seems like isolation and security can be built on top of simple
hardware feature (NOT virtualization), but we failed on doing so,
so now it seems like it is safer to use virtualzation to isolate stuff.

The descriptions on how the vm starts up fast just mapping the host kernel
as the vm kernel, bypassing memory pages etc, does not make me feel safe,
actually scares the hell out of me. It seems like a set of pretty cool hacks
involving kvm, but I would have 0 confidence on saying it is safe (there is a lot
of jumps to addresses and assumptions and memory maps being disabled...etc).

The main basis for thinking that this is a good idea is that the hypervisor
is safer than the operational system. For example the paper is always talking
about how when you compromise the operational system you compromise all containers.
But the same applies to hypervisors as well, so it seems only a matter to
decide which one is simpler and safer. Virtualization is never simple to me, isolation
seems like a subset of the virtualzation problem..so I fail to understand why
hypervisors and virtualization are conceptually safer than pure isolation
implemented on an operational system.

The whole thing seems like Intel trying to couple the idea of isolation
to their hardware features so you will depend even more on their hardware.
It may seem as a good idea just because of a total failure from operational
systems =(. Yet, I'm pretty sure that I'm going to see a lot of usage
of this kind of technology, perhaps that is why I'm writing this...it is like
a pre-rant on the thing =P.