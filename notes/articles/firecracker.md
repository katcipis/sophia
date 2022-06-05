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
and related to overall core design of Linux.

One example is the complexity of the system, the more system calls and the more
subsystems the more you have API surface to leak things. Quoting the authors:

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
The situation is so dire to solving this at the kernel that a reasonable solution
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

And then the ultimate failure, the kernel does such a bad job at
providing isolation that in order to have **just** isolation you need
to use virtualization. I get that it ends up being a "good" solution because
hypervisors do a better job at isolation, but it is a hacky solution nonetheless
and Firecracker is an example of that.

How is it hacky ? Not necessarily in a bad way, it is a very smart workaround
for a terrible environment (Linux + isolation), but the design of Firecracker
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
the very essential required just to run programs built for Linux:

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
