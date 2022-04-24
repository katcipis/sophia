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
* [Are Kubernetes CPU limits bad?](https://medium.com/inside-sumup/are-kubernetes-cpu-limits-bad-a04430bf54e1)
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

A [comment](https://news.ycombinator.com/item?id=24381813) from [Tim Hockin](https://github.com/thockin) on a
[Kubernetes: Make your services faster by removing CPU limits](https://news.ycombinator.com/item?id=24351566):

```
Since this started by citing me, I feel somewhat obligated to defend my guidance.
I stand by it.

In an ideal world where apps are totally regular and load is equally balanced and
every request is equally expensive and libraries don't spawn threads, sure.

Maybe it's fine to use limits. My experience, on the other hand, says that most apps
are NOT regular, load-balancers sometimes don't, and the real costs of queries are often unpredictable.

This is not to say that everyone should set their limits to `1m` and cross their fingers.

If you want to do it scientifically:

Benchmark your app under a load that represents the high end of reality.
If you are preparing for BFCM, triple that.

For these benchmarks, set CPU request = limit.

Measure the critical indicators. Vary the CPU request (and limit) up or down
until the indicators are where you want them (e.g. p95 latency < 100ms).

If you provision too much CPU you will waste it. Maybe nobody cares about p95 @50ms vs @100ms.
If you provision too little CPU, you won't meet your SLO under load.

Now you can ask: How much do I trust that benchmark? The truth is that accurate benchmarking is DAMN hard.
However hard you think it is, it's way harder than that.
Even within Google we only have a few apps that we REALLY trust the benchmarks on.

This is where I say to remove (or boost) the CPU limit. It's not going to change the scheduling or
feasibility. If you don't use it, it doesn't cost you anything. If you DO you use it it was either
idle or you stole it from someone else who was borrowing it anyway.

When you take that unexpected spike - some query-of-doom or handling more load than expected or
... whatever - one of two things happens. Either you have extra CPU you can use, or you don't.
When you set CPU limits you remove one of those options.

As for HPA and VPA - sure, great use them. We use that a LOT inside Google. But those don't act
instantly - certainly not on the timescale of seconds. Why do you want a "brick-wall" at the end of your runway?

What's the flip-side of this? Well, if you are wildly off in your request, or if you don't
re-run your benchmarks periodically, you can come to depend on the "extra". One day that
extra won't be there, and your SLOs will be demolished.

Lastly, if you are REALLY sophisticated, you can collect stats and build a model of how much
CPU is "idle" at any given time, on average. That's paid-for and not-used. You can statistically
over-commit your machines by lowering requests, packing a bit more work onto the node, and relying
on your stats to maintain your SLO. This works best when your various workloads are very un-correlated :)

TL;DR burstable CPU is a safety net. It has risks and requires some discipline to use properly, but
for most users (even at Google) it is better than the alternative. But don't take it for granted!
```

There is probably some scenario where a limit may be required, but it for sure
should not be a default/"best practice".
