# Firecracker: Lightweight Virtualization for Serverless Applications

Notes from the [paper](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf).

Overall the approach explained on the paper is very sensible, they define well
where they want to get and assess different strategies to get there and why
they ended up choosing virtualization as the basis.

The problem being solved is also well explained, it is not that hard to define
but it is considerably hard to solve, they want to run true multi tenant workloads.
What I mean with true ? Each tenant may be a different client, they should never
be able to see any data from each other, now that someone else is even there or
have their performance affected by other workload (which is different when the
multiple tenants are just different teams of the same company).

This multi-tenant engine is required by AWS products like Lambda and Fargate.
They run small (also big, but usually small) workloads completely isolated
from each other in a "serverless" manner. The main drive from the research
(just as it was with Borg on Google) is cost reduction. Of course making
things safer is always desirable, but they focus more on high-density
and over subscription of resources, they get to astonishing values like
10x over subscription without significant QoS loss (there is always some
for sure).

The authors go on showcasing something that I have been feeling for years
already. Linux containers are broken for any serious requirement on isolation,
and there is a good chance this will never change since the problem is systemic
and related to the core design of Linux.

One example is the complexity of the system, the more system calls and the more
subsystems the more you have API surface to protect. Quoting the authors:

```
Tsai et al [57] found
that a typical Ubuntu Linux 15.04 installation requires 224
syscalls and 52 unique ioctl calls to run without problems,
along with the /proc and /sys interfaces of the kernel. 
```

On top of the complex API surface there is also a lack of common abstraction
to isolate things, you have multiple namespaces:

* Filesystem
* Networking
* User
* Process

And probably more namespaces that I'm forgetting. You need to ensure all of them
are properly isolated since any of them may help leak information.
The situation is so dire that a reasonable solution
is to re-implement functionality on user space, which I can't shake off the feeling
of being the ultimate defeat of whatever is being done on the kernel:

```
One approach to this challenge is to provide some of the
operating system functionality in userspace, requiring a significantly
smaller amount of kernel functionality to provide
the programmer with the appearance of a fully featured environment.

Graphene [56], Drawbridge [45], Bascule [6] and
Google’s gvisor [15] take this approach.
In environments running untrusted code, container isolation is not only
concerned with preventing privilege escalation, but also in preventing
information disclosure side channels (such as [19]), and preventing
communication between functions over covert channels. The richness of
interfaces like /proc have showed this to be challenging
(CVE-2018-17972 and CVE-2017-18344 are recent examples).
```

Another example of the  kernel doing a bad job at
providing isolation is that in order to have **just** isolation you need
to use virtualisation. I get that it ends up being a "good" solution because
hypervisors do a better job at isolation, but it is a hacky solution nonetheless
and Firecracker is an example of that.

How is it hacky ? Not necessarily in a bad way, it is a very smart workaround
for a terrible environment (Linux doing isolation/multi-tenancy),but the design of Firecracker
itself makes the hackiness explicit. Even though it uses virtualisation it can't
actually virtualise different archs/OS's, the whole point of Firecracker is
to use the bare minimum of virtualisation just to get isolation and good enough
performance, ignoring/removing as much features related to actual virtualisation
as possible:

```
Firecracker is probably most notable for what it does not offer,
especially compared to QEMU. It does not offer a BIOS,
cannot boot arbitrary kernels, does not emulate legacy devices nor PCI,
and does not support VM migration.

Firecracker could not boot Microsoft Windows without significant
changes to Firecracker. Firecracker’s process-per-VM model
also means that it doesn’t offer VM orchestration, packaging,
management or other features
```

And that sums up perfectly the core idea of Firecracker, strip down QEMU to
the very essential required just to run programs built for Linux/x86/x64:

```
QEMU contains > 1.4 million lines of code, and can require up to 270 unique
syscalls [57] (more than any other package on Ubuntu Linux 15.04)

Firecracker’s approach to these problems is to use KVM
(for reasons we discuss in section 3), but replace the VMM
with a minimal implementation written in a safe language.
Minimizing the feature set of the VMM helps reduce surface
area, and controls the size of the TCB. Firecracker contains
approximately 50k lines of Rust code (96% fewer lines than
QEMU), including multiple levels of automated tests, and
auto-generated bindings.
```

So on one side you have people re-writing Kernel functionality
on user space (GVisor, etc) and on the other people using virtualisation
to desperately get some proper isolation without much overhead
(Firecracker, Intel Clear Containers, etc).

Using the bare basics of KVM + smart hacks to speed up boot time
produces impressive results, at least for virtualised solution:

```
With the provided minimal Linux guest kernel configuration,
it offers memory overhead of less than 5MB per
container, boots to application code in less than 125ms, and
allows creation of up to 150 MicroVMs per second per host
```

Memory overhead is fundamental to achieve high-density.
When discussing trade-offs, usually hardware is regarded
as safer and better at isolation:

```
From an isolation perspective, the most compelling benefit is that it
moves the security-critical interface
from the OS boundary to a boundary supported in hardware
and comparatively simpler software
```

Except when it is not :-):

```
Architectural and micro-architectural side-channel attacks
have existed for decades. Recently, the publication of Meltdown
[34], Spectre [30], Zombieload [49], and related attacks
(e.g. [2, 37, 54, 58]) has generated a flurry of interest
in this area, and prompted the development of new mitigation
techniques in operating systems, processors, firmware
and microcode.
```

So in the end securing something is considerably complex these days. On the
level of the hardware their approach was to disable hardware features
that optimize performance in detriment of isolation guarantees:

```
This migration was mostly uneventful, but
we did find some minor issues. For example, our Firecracker
fleet has Symmetric MultiThreading (SMT, aka Hyperthreading)
disabled [52] while our legacy fleet had it enabled.

Migrating to Firecracker changed the timings of some code, and
exposed minor bugs in our own SDK, and in Apache Commons HttpClient [22, 23]
```

Which shows how tricky is to get good isolation even on the hardware level +
showcases yet another bug caused by changes on timing/concurrency/etc :-).

Since this is a battle fought at all layers, on top of Firecracker they also
added a Jailer using isolation features from the kernel:

```
Firecracker’s jailer implements an additional level of
protection against unwanted VMM behavior (such as a hypothetical
bug that allows the guest to inject code into the VMM).

The jailer implements a wrapper around Firecracker which
places it into a restrictive sandbox before it boots the guest,
including running it in a chroot, isolating it in pid and network
namespaces, dropping privileges, and setting a restrictive seccomp-bpf profile.

The sandbox’s chroot contains only the Firecracker binary, /dev/net/tun,
cgroups control files, and
any resources the particular MicroVM needs access to (such
as its storage image). The seccomp-bpf profile whitelists 24
syscalls, each with additional argument filtering, and 30 ioctls
(of which 22 are required by KVM ioctl-based API).
```

In this case it is easier to isolate since you are always running the same
application (Firecracker) and you have good knowledge of which resources/syscalls
it needs (running any arbitrary program correctly is a harder problem).

So in the end you have Linux Containers on top of KVM on top of carefully
configured hardware :-). This reminds me a lot of 
[Modules, monoliths, and microservices](https://tailscale.com/blog/modules-monoliths-and-microservices):

```
By far the part we're worst at is #1, isolation. If we could truly and
efficiently isolate one bit of code from another, the other goals would
mostly fall into place. But we simply do not know how.

Isolation is a super hard problem. Goodness knows people have tried.
Yet browser sandbox escapes still happen regularly, undetected privilege
escalation attacks are simply assumed to exist on every OS, iOS still gets
jailbroken periodically, DRM never works (for better or worse), virtual
machines and containers regularly have vulnerabilities discovered, and
systems like k8s have their containers configured insecurely by default.

People have even been known to figure out encryption keys on remote
servers by sending well-timed packets to them over the Internet.

Meanwhile, the most spectacular isolation failures in recent memory were
the Meltdown and Spectre attacks, which allowed any program on a computer,
even a javascript app in a web browser, to read the memory of other programs
on the same computer, even across sandboxes or virtual machines.
```

And that is a struggle I have observed my whole life as I build software.
Isolation is an essential property to design software at scale, and yet for a
lot of different reasons it is remarkably hard to achieve.

Going back to Firecracker, in the end the system worked pretty
well and the migration is strategy very sound:

```
We took advantage of users of Lambda inside AWS by
migrating their workloads first, and carefully monitoring their
metrics. Having access to these internal customer’s metrics
and code reduced the risk of early stages of deployment,
because we didn’t need to rely on external customers to inform
us of subtle issues.
```

Always eat your own dog food :-).

## DNS Fun

Since I always had my share of problems that seemed quite serious and in the
end was just some DNS caching missing (thanks Azure) I was thrilled to see
I'm not the only one:

```
Our deployment mechanism was also designed to allow
fast, safe, and (in some cases) automatic rollback, moving
customers back to the legacy stack at the first sign of trouble.
Rollback is a key operational safety practice at AWS, allowing
us to mitigate customer issues quickly, and then investigate.

One case where we used this mechanism was when some
customers reported seeing DNS-related performance issues:
we rolled them back, and then root-caused the issue to a
misconfiguration causing DNS lookups not to be cached inside the MicroVM
```

## Immutable Infrastructure

Since I always push for Immutable infrastructure, I felt compelled to
quote them on this:

```
We use an immutable infrastructure approach, where we patch by completely
re-imaging the host, implemented by terminating and re-launching our AWS
EC2 instances with an updated machine image (AMI).

We chose this approach based on a decade of experience operating AWS services,
and learning that it is extremely difficult to
keep software sets consistent on large-scale mutable fleets (for
example, package managers like rpm are non-deterministic
producing different results on across a fleet).
```

## Configuration

Firecracker is configured through a very simple REST API. Which is a nice
change to the usual complex configuration setups, or using some fancy/specific
RPC thing.

## Conclusion

In the end, given a bunch of wrong reasons, something like Firecracker seems
like a good idea, and it seems they were always very concerned about making things
as simple as possible, both on runtime overhead but also on amount of
code/complexity. It was really nice to find out that the project itself is
open source, seems like a good place to take a look into for some solid
Rust code examples.
