# Large-scale cluster management at Google with Borg

These notes are based on the paper [Large-scale cluster management at Google with Borg](https://research.google.com/pubs/pub43438.html).

# The Problem

What was the problem being addressed by google when they developed Borg ?
Basically Google had a massive datacenter and desired to put it to good use.
The whole engineering behind borg was economic. Do more with less, use well
the hardware available.

Of course that is not the whole motivation, but along the paper good part of the
problems solved revolved around this. Another motivation was to provide a good
abstraction on how to deploy software easily on such massive scale. When you had
hundreds of teams deploying thousands of services, how do you scale that ?
(which is also an economic factor)

What do I mean by scale ? Well even on the scale of dozens, manually scheduling which
services runs where is a tedious job and prone to human error. Failing on scheduling
the processes around the machines on your cluster may cause downtime on critical services,
poor user experience or leave resources idle.

Without any kind of abstraction every developer deploying software would have to look
the resource usage of all hosts and decide where to run its new service. Besides this
there is also the fact that resource usage accross multiple machines can oscilate greatly
as usage patterns changes on the client side. The developer would face the same challenges
when trying to scale its service in face of a higher user demand, even worse, the developer
would have to always manually intervein on the system when users demand more from
a specific service. This is not feasible if you desire to respond fast to higher
user demands of a specific service.

The most simple but inneficient way to handle these problems is to over provision
computational resources and under utilize them, this way if a service needs more computational
power it can get it easily since the resource is already laying there idle.
Basically hosting a few services on a huge computer and leave a lot of CPU and memory
to spare. On a small scale this can be a more economic choice, but on the scale of Google
this can translate to millions of dollars os waste. So investment on better engineering
made sense. Some goals of Borg that I extracted are:

* Uniform deployment model
* Scheduling (and rescheduling) of services accross a cluster
* Provide high availability for critical services in face of hardware failures
* Optimize as much as possible for high utilization of resources (no idle resources)
* While trying to optimize for high utilization never jeopardize the user facing low latency services

The problem is pretty hard because some of these forces opose each other, so
balancing them is not trivial (like high utilization VS quality of service).

# Uniformity

One of the goals is the desire for some uniformity and consistency on the deployment
(what people these days do with docker for example), which Google attacked simply
by defining that all deployments consisted from a static binary (the application)
and N number of files that where assets to the application (like configuration,
or CSS/HTML if the application provided these kind of things).

They normalized all deployables to this format, lot of detail is missing on the paper but it makes
sense to use static binaries to make deployment easier. When compiling static binaries
on Go the use of Docker (or containers) seems almost irrelevant, unless you have some
assets you need to package with the binary (embedding on the binary is always an option but
it can get pretty weird pretty fast depending on how much assets are required).


# High Availability VS High Utilization

High utilization is a term used often on the article, it is used to refer to the idea
of using resources of machines as much as possible. For example, if a machine has 16GB
of RAM, using 4 GB could be considered low utilization while using 15.5 GB would be
high utilization. It is basically the idea os squeezing everything together as much
as possible.

How this goes against high availability ? Lets say you have a service that at peak
usage needs 10GB of RAM (Java stuff probably =P), but on almost all the time it
used only 1GB of RAM. To guarantee high availability you need to reserve 10GB of
RAM, so when you need it you can expand fast and handle the high peak. But to guarantee
this the reserve forces you to let the memory idle, so when you need it is there.
In that sense, the more you want to GUARANTEE a critical service more you will need
to waste resources to do it (circuit switching VS packet switching networks have
similar tradeoffs).

So how this is attacked on Borg ? They created two classes os services, batch jobs
and low latency services. And within these two classes a numerical system or priorities.
For what is those classes and priorities used ? To determine when a job will be evicted
from a host so availability is not impaired on a sensitive critical service.

How does that work ? Basically:

* Critical service X claims that it required 10 GB of RAM
* Borg runs it
* Borg collects runtime information on resource usage on all hosts
* Borgs sees that service X is actually only using 1 GB of RAM almost all the time
* Batch job Y claim that it requires 9 GB of RAM
* Borg runs Y on the same host as X, allowing Y to use reclaimed resources
* Reclaimed resources are resources reclaimed by some process but not being used yet
* As service X starts to use more memory Borg may decide to evict Y and run it on other host

The algorithm is much more complex than this, this is a very simple overview that I got.
It is a lot of complexity, but maturing it allowed google to use resources much more
efficiently, at least the paper presents this as a fact. Some tests have been made
running the batch jobs on a different cluster form the low latency sensitive services,
but this required 30% more hosts than running everything together, so the investment
seems to pay off for google scale.

But this algorithm shows how important is to Borg to run applications completely
isolated from each other and have great control on how much resources each one can use
(just like Kubernetes, Borg also has resource limits that are used to kill misbehaved
processes). So how does Borg approaches isolation ?


# How to Isolate ?

Sadly the landscape of isolation on operational systems is so poor that virtualization
is often seem as a good approach to do it, even when virtualization itself is not
necessary. This option was also analized by Google engineers, and the rationale on why
they chose to not use it is very interesting:

```
The  vast  majority  of  the
Borg workload does not run inside virtual machines (VMs)
because  we  don’t  want  to  pay  the  cost  of  virtualization.
Also,  the  system  was  designed  at  a  time  when  we  had  a
considerable investment in processors with no virtualization
support in hardware
```

For me this show so much consideration for your own problem at hand
and even details of your hardware, instead of searching for canonical
best ways to solve something. When thinking on solving a generic problem
for cluster orchestration perhaps virtualization would be a good idea,
specially on a time where there where no "containers". To avoid use
of virtualization they had to use cgroups + chroot to achieve
some level of isolation required to make Borg work properly (remember,
low priority jobs may never get on the way of high priority ones).

This is the lesson that I see passing unaware by people that want to be
like Google. You could argue that you don't have to be like Google, because
you are not Google, and you are probably right. But the problem is that being
like Google, in your own scale, it is a good thing. Because being like Google
is basically solving your problems the best way for you, instead of using
code from other companies or open source groups that are made to fit everyone
needs (and usually satisfying none in the end). So if there is a lesson to
be learned by studying the development of Borg is to not use Kubernetes
if you think that something simple that you can rollout will be enough
for your needs, or will fit better your case.

You may think that you can't do what Google does because you don't have
their scale, and again you are probably right =). But since you don't have
Google's scale the software you need to right to solve your own problem is
smaller, so you can do it even when you don't have Google scale, because
you are 1000x smaller and need 1000x smaller code too, so the modus operandi
seems to apply on any scale. Of course this does not mean that you need to implement
everything from scratch, but these days the balance seems to be much more prone
to always vendoring code, almost always embracing more complexity than you need,
or something that is not tailored for your particular problem. On this sense I find
Borg pretty awesome, it is extremelly tailored for their use case.

Going back to how isolation is achieved, chroot + cgroups and other kernel
features to guarantee isolation and to guarantee that processes don't use
more memory than they are allowed. To give a high quality of service on this
model you depend a lot on the kernel since if it fails to kill a process using
too much memory this will interfere on the availability of other services.
Achieving this was not trivial on linux (is there anything that is ?), but it was
possible (there are entire researchs dedicated just for this topic on linux).

# Networking

Another thing that is never easy to handle on a distributed system is networking
and service discovery. One of the things that where really hard to understand on
Kubernetes for me was the networking model. There can be 3 different networks
on a cluster. I remember to be a little upset with this complexity, but Kubernetes
docs where pretty clear on explaining why, and also does the Borg paper.

The way Kubernetes approaches networking, with one IP address dedicated to each running
pod (a job on Borg, aproximatedely) is a lesson learned from Borg. On the beggining of the
project it was clear that the scheduler and orchestrator was the best one to also handle
service discovery, since it knows where every service is running, but no additional network
layer where added, the jobs use the host IP addresses, which introduces a pretty hard
problem to solve, which is port collision.

If you choose to go this way, Borg does pretty sensible choices. Basically every job that
needs to listen on a port must declare this on its config, just the need for ports not the
specific port, since they can't choose it. Borg will schedule ports to the jobs dinamically
and inject them on the jobs through environment variables. Every job must read this environment
variables to know which port it can actually use. Scheduling the ports is also not trivial.
On their view this was a mistake, the complexity of this port handling logic justified
introducing the complexity of SDN's (Software Defined Networks) in order to give control
of which ports to use back to jobs (and relief the orchestrator from this responsibility).

DNS is "solved" with a simple globaly distributed file with all the endpoints of running
services, a lookup on this file is the service discover. There are layers on top of that
to work with the DNS protocol itself too.

# Debugging

One other thing that got my attention is how bugs on Borg are debugged. Since Borg
can orchestrate clusters of 10000 hosts its internal state can get pretty messy
and reproducing live production bugs can be a challenge. How did they approach this ?
Borg generates an event log of everything that happens on the cluster. With this log
in hand (not a debug log, something structured for machine consumption) they developed
fake Borg master (much like Kubernetes master) called fauxmaster that consumed this
log to reproduce the same state of the real borg master on production. All the code on
the fauxmaster is exactly the same of the borg master, only the RPC calls to the
borglets (analogs of the kubelets) are mocked. This way you can rewind the fauxmaster to
any point on time and experiment with it to see if it will achieve some catatonic state.

This is also very useful to capacity planning, it is very normal to load a log like that
and them try to run new services on the fake cluster and see if the scheduler will be
able to fit new jobs on the cluster or not, but all this running with mocked borglets.
It is a pretty good idea to apply on complex distributed systems (or even simple ones).
Feed a new state machine with all the events and you will have an exact copy of the
one running on production. Of course this will usually not be 100% true since it requires
a lot of discipline and determinism to do this (things that are usually lacking on software
development), but it seems to work well enough to be proven useful on a lot of cases.