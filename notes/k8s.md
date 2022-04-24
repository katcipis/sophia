# Kubernetes

## CPU limits

I have a beef for years with CPU limits, not because they are intrinsically bad,
but because for some confusing reason it started being proposed as a "best practice"
to avoid workloads hogging the CPU and starving other processes on the host.

It is a good idea to prevent starvation, the problem is that imposing limits
will solve that but at the cost of creating an artificial throttling that may
impact negatively your application.

The proper solution is to always define a CPU **request**. When it you request
some CPU, Kubernetes will do its best to guarantee that you get at least
how much CPU your requested, there is no way for other process (running inside k8s)
to steal that CPU time from the process that requested it. The whole thing is
not perfect, since containers overall (cgroups, CFS Scheduler, etc) are not perfect,
but they offer the same level of guarantee that limits do (which are also not perfect
since they depend on the same mechanisms to be enforced).

Double checking this on [Kubernetes docs](https://github.com/kubernetes/design-proposals-archive/blob/8da1442ea29adccea40693357d04727127e045ed/node/resource-qos.md#compressible-resource-guarantees):

```
Pods are guaranteed to get the amount of CPU they request, they may or may not
get additional CPU time (depending on the other jobs running).

This isn't fully guaranteed today because cpu isolation is at the container
level. Pod level cgroups will be introduced soon to achieve this goal.
```

So if you have troubles were some pod is not getting enough CPU, it is probably
because you didn't requested enough CPU, not that some limit is missing somewhere
else. Limits will also "solve" the problem, but with a higher probability of
causing latency issues depending on the limit + scheduler window configured on
Kubernetes.

As far as CPU requests are defined the CPU limit only affect how spare
CPU is distributed. So it is not the actual solution to CPU starvation.

Another issue that people try to "solve" with CPU limits is setting limits==requests
so the Pod has Guaranteed QoS, so it will not be the first to be unscheduled
(also scores better on OOM events).  Pod disruption budgets are the proper
solution to most of this, not QoS, specially since
when people apply this as a "best practice" you end up with a lot of Guaranteed QoS
pods and hence you will have no special QoS to any pod, since they are all
Guaranteed anyway. I haven't used much but would also look at the prioritization
model that was added on k8s after the whole QoS classes thing, it seems
much more explicit/sane.

Some more information:

* [For the love of god, stop using CPU limits on Kubernetes](https://home.robusta.dev/blog/stop-using-cpu-limits/)
* [Challenges of monitoring CPU limit issues](https://github.com/robusta-dev/alert-explanations/wiki/CPUThrottlingHigh-(Prometheus-Alert))
* [CockroachDB on k8s docs](https://www.cockroachlabs.com/docs/stable/kubernetes-performance.html#resource-requests-and-limits)

The warning on CockroachDB docs:

```
Warning:
While setting memory limits is strongly recommended, setting CPU limits can
hurt tail latencies as currently implemented by Kubernetes. We recommend not
setting CPU limits at all unless you have explicitly enabled the non-default
Static CPU Management Policy when setting up your Kubernetes cluster,
and even then only setting integer (non-fractional) CPU limits and memory
limits exactly equal to the corresponding requests.
```

There is probably some scenario where a limit may be required, but it for sure
should not be a default/"best practice".
