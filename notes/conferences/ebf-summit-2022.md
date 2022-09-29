# eBPF Summit 2022

# Cilium Standalone XDP L4 Load Balancer

Presented an interesting use case where they have their own datacenter/load
balancing infra.

The load balancing used to be done with IPVS and it was migrated to
the Cilium XDP L4 Load Balancer.

Got more than 10x improved performance (CPU usage/packets per sec).

# When you need to overcome your fear and build your own data-driven eBPF firewall

They wanted to implement a firewall using dest/src IP + src MAC as the match
to allow/deny traffic. The guy went on in details on how to do that in eBPF
using a simple eBPF map.

I particularly liked the solution since they extracted the eBPF logic in a very
simple C program and the rest of the interface manipulating the firewall rules
were Go programs which then provided more sophisticated interfaces (the control plane).

One of the few presentations where in the end it was talked about downsides of eBPF.
In their case debugging was quite hard in some cases, sadly he didn't provided a lot
of detail on why it was hard.

# eBPF for IO latency monitoring

Showcased Digital Ocean's usage of eBPF to collect IO latency information and
exporting it to Prometheus. Basic eBPF program providing histogram on the map
and then more high level code getting that data and exporting it on Prometheus.

Also talked about some challenges on eBPF. In this case changes on tracepoints
and structures on the Kernel. CO-RE and BTF are supposed to help with this but
I'm not sure how far it can help, depending on the change on a tracepoint I suppose
it is inevitable to see a break. Sadly it was quite quick and no details were
provided.

# Improving Cilium's eBPF-based DSR performance by adding support for IP-in-IP

This one was quite interesting because the guy makde changes on Cilium load balancer
to allow for more performant DSR load balancing. In order to get fast paths on
switches he used IP-in-IP, encapsulating the original package info on a SNAT'ed package.
So you get the performance of doing the SNAT but the end result is still DSR.

# The Promise of eBPF For The Edge

There was nothing very new/extraordinay on this presentation to me other
than the fact that Red Hat seems to be investing a lot on eBPF in general.
Including a Rust library, which seems interesting:

- https://github.com/aya-rs/aya
