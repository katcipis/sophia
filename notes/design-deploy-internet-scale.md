# On Designing and Deploying Internet-Scale Services

[Original article](https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton_html/)


## Highlights


### Holistic

```
Throughout the sections that follow, a consensus emerges
that firm separation of development, test,
and operations isn't the most effective approach in the
services world.
The trend we've seen when looking across many services
is that low-cost administration correlates
highly with how closely the development, test,
and operations teams work together.
```

Makes sense and the industry is going on this direction using stuff
with pretty names like TDD and DevOps.


### Constraints

```
This is a repeating theme throughout: simplicity is the key to efficient
operations. Rational constraints on hardware selection, service design,
and deployment models are a big driver of reduced administrative costs
and greater service reliability.
```

Sadly I have to agree with this, although there is a middle ground to be found
on this. Why sadly ? Well I  dont know one good hacker that likes to be
constrained, we have aversion for the word constrain.

But maintaining a sea of different hardware + designs + deployment models
can converge to a raise on the costs of maintaining it. A set of good
practices and shared knowledge on how to do stuff seems like a good idea,
even with the devops idea of every team doing things independently.

This can be seem more clearly when you think about the impact of
team members shifting through the organization + team members leaving.

I had some experience with single deployment model, using containers, and
I have seem a great deal of advantages on this, instead of the traditional
different deployment model for each language that you use. It makes sense.
Containers enable people to experiment with tools and language without
incurring on high operational costs, that is cool.

Of course that creating a culture where no one can experiment and try
different stuff will guarantee failure too, since no one interested on
learning and growing will want to work on a place where everyone is
obligated to do everything like everyone else.

There is a lot of places where experimentation has low impact, like
development environments. Other ones requires a more stable approach, like
databases that are being used on production, and infrastructure responsible
for keeping stuff working.


### Chaos

```
If the failure paths aren't frequently used, they won't work when needed
```

On testing (unit and integration) I have been able to give attention for
this idea, but on production with the live services it is still a maturity
level that I can't reach easily. But it makes total sense, this is why people
do fire drills, you cant wait for a actual fire to discover if people will
survive or not :-).

Here enters all the Chaos stuff from Netflix, for example. Developing backup
and recovery plans and never exercising them is like coding without tests.
You have no proof at all that it actually works, only hope, and hope is
not a good way to do engineering.


### Cynicism

```
Zero trust of underlying components. Assume that underlying components will
fail and ensure that components will be able to recover
and continue to provide service
```

Another reference to this idea is [ROC](http://roc.cs.berkeley.edu/).
For me it is the idea of embracing uncertainty and failure. You cant avoid it,
so use your energy to recover from it.

The idea is to degrade service gracefully, instead of locking or exploding in
a million pieces. Even when you cant degrade the service, failing fast
is a great option, instead of just locking for long times and giving odd
slow errors to clients.


### Automate

```
Rather than documenting these as multi-step, error-prone procedures,
write them as scripts and test them in production to ensure they work.
What isn't tested in production won't work, so periodically the operations
team should conduct a "fire drill" using these tools
```

Every time you are documenting a series of commands to be executed on a shell,
think "Why this cant be a script ?". I has my share of documentation on wikis,
with lots of commands, it is just plain stupid. Documentation is always better
as code. It is formal, exact and you can execute it without space
for human errors. It scales much better.


### Simplicity

```
Simple and nearly stupid is almost always better in a high-scale service-the
number of interacting failure modes is already daunting before complex
optimizations are delivered. Our general rule is that optimizations that
bring an order of magnitude improvement are worth considering,
but percentage or even small factor gains aren't worth it.
```

Nothing to comment on this, it is good advice. But it is surprising how
we dont take this in consideration and start with fairly complex solutions.


### Failing

```
The general rule is to attempt to gracefully degrade rather than hard failing
and to block entry to the service before giving uniform poor
service to all users.
```

This reminds me of the bulkheads idea from
[Release it](https://pragprog.com/book/mnee/release-it). If possible it
is good to degrade gracefully, providing a more essential service and
stopping to provide a less critical one in face of some failure.

Separating stuff on different services will not solve this completely.
That is why some cluster orchestration tools like Kubernetes have nice
APIs to define QOS (Quality Of Service) configuration, so you can guarantee
that problems on a less critical service wont affect a critical one.

If this is not possible, at least stop accepting new requisitions.
Failing doing that will guarantee that ALL clients will have a poor experience.


### Stateless

```
Prefer stateless implementations. Don't affinitize requests or clients to
specific servers. Instead, load balance over a group of servers
able to handle the load
```

Sometimes this is not very easy to implement, it seems easier to use things
like sticky sessions, but usually it pays off greatly :-).


### Replication

```
A system designed for automation pays the latency and
throughput cost of synchronous replication.
```

Recovering from failures on databases, specially when they are huge,
it is not easy. The idea here is to prefer synchronous replication,
this will make you pay in performance, but you will be able to have
a fully automated recovery system.

When replication is asynchronous you will always have to make a manual
choice of "is the replica data good enough" when recovering a failing system.
This decision can get pretty complicated depending on the business.

There is a tradeoff here. Since the prize of doing it synchronously is
full automation, it seems to be a good idea when applicable.


### Configuration

```
Services that treat configuration and code as a unit and only
change them together are often more reliable
```

Greatly approve this idea. Configuration usually is abused and treated
as a second class citizen. It is just configuration,
you can just go there and change it in production manually.

Changing configurations have the same impact as changing code, it breaks stuff.
There is no clear separation between them for me.

And lately I even like to use code
to express the configuration, place it together with the code,
version everything together, and any change on one or another
requires a new full deployment.


### Timeouts

```
Ensure all interactions have appropriate timeouts to avoid
tying up resources for protracted periods.
```

Plain and simple. Timeouts are basic.


### Testing

```
Testing in production is a reality and needs to be part of the quality
assurance approach used by all internet-scale services.

Most services have at least one test lab that is as similar to production as
(affordably) possible and all good engineering teams use production workloads
to drive the test systems realistically. Our experience has been, however,
that as good as these test labs are, they are never full fidelity.

They always differ in at least subtle ways from production.
As these labs approach the production system in fidelity,
the cost goes asymptotic and rapidly approaches that of the production system.
```

Nothing beats up testing on production. This is scary at start but it will put
you on the good path of building resilient services that can withstand tests
on production and also make you think on tests that can run on this environment.

The more scary scenario is when you have databases. What if I do something that
fucks all my data ? Well, that already can happen on your production
environment, how you recover from this kind of error ?

The usual response is that it cant be properly recovered, which is sad.
When you solve this, testing in production becomes feasible. Of course it is
not trivial, but it is not a problem that can be avoided.

Also it is a great way to cut operational costs.


### Alerting


```
People stop paying attention very quickly when the data is incorrect.
It's important to not over-alert or operations staff will learn to ignore them.

This is so important that hiding real problems as
collateral damage is often acceptable.
```

In testing this is a old idea, avoid false positives. But on alerting people
often get excited about the possibilities.

It is much better to even hide production problems than over alerting.
Alerting will render your monitoring solution useless, just as false positives
on tests render a test suite useless.

A good read on the subject can be found
[here](https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/edit#heading=h.fs3knmjt7fjy)
