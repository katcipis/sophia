# Software Reliability Engineer

These notes are inspired on the great talk [Keys to SRE](https://www.youtube.com/watch?v=n4Wf14e2jxQ)
from Ben Treynor, but they are more than just notes from the talk since reliability and
resilience is a subject that I always enjoyed brainstorming about.

The idea of a software engineer focused only on reliability is part of a greater idea
on how to approach reliability on Google, but before going on the specifics on how this works
at Google it is interesting to think about the forces revolving around reliability and the
trade-offs involved.


## Trade-Offs

There is a lot of examples on software development of opposing forces that are both
desireable and you will have to choose how much of each one you will desire, like for each
decision you make you need to understand which one you are favoring.


### Scalability VS Consistency

The more classical one is high availability and scalability VS consistency. The Internet
gives a great example on how these forces interact when you think about caches and CDNs
(Content Distribuition Networks). Scaling video streaming worldwide is extremelly hard, without
some sort of distribuition network + caches scaling this would be very expensive, and providing
low latency could be plain impossible (physical limitations). But the same architecture that will
enable scaling and distribution of the data will make it almost impossible to make the system consistent.
If a video is changed the chances that people around the world will see different versions is enourmous.
The problem here would be cache invalidation, which to implement in a consistent way would render a system
almost impossible to scale (with acceptable latency) on the level of the Internet (worldwide),
so you need to choose if you want this kind of scale/low latency or consistency.


### QOS VS High Utilization

Another interesting example is quality of service (like low latency) VS high utilization of resources.
The more agressive you are about sharing resources between different services you will be able to use
resources better (no idle CPU or memory for example) but harder it will be to maintain good quality
of service (latency/response time/etc) if there is a spike of usage on a specific critical service.
It is very easy to understand this when you think about a non-multiplexable resource such as memory (networking
and CPU can be sliced as much as you want in the time domain, memory can't).

Lets say service A needs 10GB RAM on its peak usage, but 90% of the time it needs only 1GB ram.
The best way to guarantee that you can handle the peak is to reserve 10GB of memory and don't
allow anyone to use it, even if it means leaving it idle, just to guarantee that it will be
available in the milisecond that you need it.

To avoid such waste of resources you could allow other services to use this memory and
when the spike of usage is presented you reclaim it back, but this will add to the latency
of your service (there is a lot about this kind of tradeoff on the 
[Large-scale cluster management at Google with Borg](https://research.google.com/pubs/pub43438.html)
paper).


### Reliability VS ???

So what is the force opposing reliability ? It seems to me that it is short-term speed.
It is a subject of great discussion how reliability can help software on mid/long run,
importante of tests and documentation, etc. But it is a fact that thinking and implementing
a resilient piece of code is much harder and takes more time than one that is not. Proof of
that is that there is more code that is unreliable than reliable ones, and achieving reliability
is hard to achieve, exactly because it is easier to just focusing on finishing a feature and
deploying it on production as fast as possible.

The more you worry and work on resilience, the more time it will take before you deploy
your code in production, that seems to be the tradeoff for me. I think a lot on the practices
that are used by SREs at Google support this idea since they strive to connect the developer
to the feedback of his decision to ignore reliability concerns to favor features. The whole idea
of trying to provide as much feedback as possible about these decisions support the idea that when
decoupled from this people usually take the faster and easier path, and the faster and easier path
is the unreliable one.

I'm more used to think about reliability coming from within the code (not at the infrastructure,
although both are necessary) and at least for code, from experience, I really believe that reliable
and well tested code is orders of magnitude harder to develop than just hacking something pretty fast
that works almost all the times (and that you have no idea how it will behave when something goes wrong
or at scale).

The idea is that both on the subject of automating operational work and on reliable/resilient code,
doing something robust is more expensive and will make you go slower on the short term. Of course that
on the mid/long-term you will actually be much faster, but human beings have problems seeing mid/long
term benefits, specially under pressure. That is why resilience is so hard to achieve (hence SRE and
DevOps, which are people striving to find good ideas on how to approach the problem, it does not come
naturally to people in general).


## Roots for Reliability

Thinking about systems and processes that I observed and have been part of and
putting it together with the approach described at the presentation I can think
about two main approaches on how to achieve reliability on a system where there will
be people extremelly interested on delivering features fast and will not be
willing to work on reliability issues. These two approaches are like roots for
a series of other derived values, principles and practices.

Both approaches needs support from the entire company, if shortsighted people
interested on features above all have power to allow people to ignore the process
for the sake of delivering something to client X tomorrow that will be nothing that will help
you, you are screwed (think about it as minimal maturity to try anything at all).

Everytime that you impose a rule there will be an exception, the problem is when making
exceptions is sistematic.


### Centralized Control and Enforcement


This is the one that I see emerge more easily, specially after trying and failing at
self regulation/police (for various reason, one of them being that it is just hard =P).
Usualy self regulation and autonomy seems to work only with certains teams, it seems tempting
to think that this is a sign for the need for centralized control and enforcement, or some form
of hybrid scheme where some teams need to be controlled while others retains autonomy.

There are some flaws at this kind of approach that I dislike, being:

- Hard to scale since it depends on a centralized/specialized team
- Weakens the liability of the team developing features to reliability problems (it is not their problem)
- It will be harder to evolve/mature the team, the tendency will be to stabilize on the "follow orders" thing

The hard to scale part is pretty easy to see since the centralized organization that
is responsible for reliability issues it will need to scale according to the amount of
teams needing it. It reminds me a lot of the classical problems with QA teams when developers don't
test their code properly, and since I saw this fail miserably it seems more scalable to
distribute this work among the teams developing the features, specially because they are
the best to attack reliability problems since they know their code better than anyone else.

The weakened liability is basically what happens when you take autonomy away from people
and don't provide clear feedback to them about their decisions, so when things go wrong it
is not their problem. You will need a set of enforced rules that guarantees reliability and
a huge police force to enforce them for this to work, because on this mode people will
be able to just do what they where told and wash their hand, so you need to get the
rules/practices right, which at least for me is not even possible since context changes a lot.

Besides that, a lot of energy will be wasted because teams that have good ownership of
their code and take pride on it's reliability will probably ignore rules if they don't agree with it,
specially when the rules are trying to be applied globally using some common denominator.

So you will be stuck with people that don't want to do things but are "obligated" to do by
some sort of ops/reliability police (they will do the minimum and make it clear that they are not liable
and where just doing what they where told) and the other group that will just do whetever they
want to do since they understand that they are responsible for the code they write and its
reliability.

In the end this model seems to be for more imature organizations that are at a smaller scale
(I cant imagine this working at a big scale, at smaller scales it may work although it still is
not ideal and a lot of energy will be wasted).

This model reminds me of the podcast
[Trust Yields Performance](http://developeronfire.com/podcast/episode-225-olve-maudal-trust-yields-performance)
Olve Maudal talks about how you will usually have something like 80% good developers and 20% of not that good
developers (unless you have a perfect hiring process, which is probably not true) and that you should focus
on principes and practices that optmizes the work of the 80% good developers and helps improve or even
fire the bad ones. Control and enforcement always seem to me as a practice that optimizes for 20% of
bad developers and annoys and get on the way of the 80% that is trying to do a good job.


### Self Regulation/Policy

This model is the one that is more in fashion right now (2018) with the name "DevOps" and is based
on the idea that each team is independent and responsible for the services they develop on an end to
end fashion. This always seemed more natural for me since ever, but in the industry for some decades
there have been a huge force on slicing the work of developing software and isolating each "phase"
of the development from each other, even being done by different teams. Something like:

* Analysis and architecting
* Coding
* Testing
* Ops

As this model not worked (for a variety of reasons, pretty extense to brainstorm about) the whole
industry started a movement back to the original model where the people writing the code handled
all these different phases together. The first part was the analysis/design/architecting and testing
with things like extreme programming. Then as expected came the desire to algo merge operational work,
hence DevOps. Not that reliability work equal ops, reliability encompasses all these different
activities of software development, but ops was the last one that was left as a separated activity
and now it is trying to be merged again (not sure if successfully given how much companies are
hiring "DevOps"...which seems to became a new name for operators that do some automation).

Even tough SRE does not seem to be only about operations, it is operation that is usually a source of
a lot of unreliability and manual/repetitive labor. The problem with this idea is not the idea itself,
since it is pretty simple on its core, which is to just expect people to do a good job, where "job"
is everything that allows you to deliver your service to the client. As mentioned before no hiring process
is perfect, and specially on programming we have more work to do than people available and it is a very
young profession, so you have a lot of well intentioned but not very experienced people (and sadly
since there is a lot of money involved a lot of bad intentioned people that is just on that for the money).

So in the end just waiting for all teams to do a good job and test/automate things properly is
not going to work at a big scale either. This is the part that makes people fallback to controlled
policy enforcement, but it does not have to be like that.


## How to Self Regulate ?


This is the core of the presentation as I understand since the whole SRE approach on
Google seems to be away to make self regulation/policy work on a huge scale. When you try
to come up with good practices and rules to how software is built you usually will hit a lot
of ambiguities and practices that don't make sense devoided of context, so the best you can
do is to come up with the smallest set of rules possible and that have as little ambiguity as possible.

Given that, the best you can do is just one rule that has no ambiguity at all and the SRE approach to
that is to have a single rule:

* If you exceed your error budget, you can't release

To make this single rule work there is a lot of pre-requisites and other practices, but
as a hard rule this is the only one that you have, if for a period of time that has been
accorded previously (SLA) your service already failed more than it should, you can't release
anymore until the time window for the error budget ends. I find this idea completely genious.

As stated before, usually when things are not being automated properly and code is not very
well tested it is because people are on a hurry for delivering features, by applying this rule
you are punishing them where it will hurt more, their ability to deliver the feature. It is a simple
"if you don't do it right you cant ship it". And there is no ambiguity/subjectiveness on "done right",
it is not about test coverage, design of the software, code reviews.

It is as simple as how much failures
is your service experiencing on production. It makes sense since all good practices and principles
in software development have as objective reliability of the software, so if you measure reliability
you have what you need to reward and punish teams. Actually thinking about punishment may not be
the best way to look at it, it is basic feedback that you are unbalanced on how you are developing
software and that something must change (punishment assumes bad intentions).

One key pre-requisite to work with this idea is very clear by now, which is basic monitoring on the
availability of different services. It makes sense to require this since on any scale that is not
very small not having this will cripple your ability to scale. Another one is adoption by all
areas of the company, specially comercial/product related ones, because when a team does exceed the
error budget the feature they desire will **NOT** be released, even if it is "done", and they will
need to be OK with that.

It is still not just that, why ? Well, perhaps a team will just keep hiring ops people to do
manual labor and all sort of crappy jobs like bug fixing and finding crappy ways to maintain
the system working. Basically people will still try to cheat their way into not using the error
budget but not using their time on reliability issues (their only concern is the features).

So what else can be done ? There is some other practices that helps balancing this kind of behavior.

The first one is that the pool of resources for developers and SREs is the same. If a product has
a budget of 10 developers this budgest is shared with SREs people, so if you want to give a crap
to reliability and just toss people to work on manual/repetitive crap you will lose developers
as you need more operational people. There is a great incentive here, as more autonomous your system
is the more you can spend on developers to implement features.

But there is another two practices that also hinder the ability to just put people doing boring ops work
to ensure reliability:

* SREs are software developers
* SRE portability

By SRE being software developers I mean developers capable of doing the exact same work
that people focused on features do, they have the same skills and desires. Why is this important ?
Well if you hire good programmers to be SRE's they are people that gets bored **VERY** easily and
will not stand to do manual/repetitive labor, and besides not wanting to do the manual stuff they
will have the skills to automate it properly.

The complementary (and extremelly necessary) practice is SRE portability, **NO** SRE will be obligated
to work on a project that he don't want to work on. If an SRE wants to move to another project, he cans,
always. This is important because if you sum it up with the fact that SRE's are just programmers if they
get bored and they can't implement the automation on the project (for a series of reasons, sometimes involving
some senior developer that just don't allow them to do their work) they just leave. So bad projects to
work will end up with no SREs resources at all, and when that happens the developers left on the project
**WILL** have to do the reliability and operational work since they are responsible for the exodus
of SRE's from their project. This can result on projects being unable to ship features from periods of
6 months to even an year. It is not a desireable outcome since it does hurts the company, but to enable
balance to be found and to people respect reliability issues it is **VERY** important that this **CAN**
happen if you ignore these issues for a long time (it must be a reality, not just a treat).

It is beautiful to see how these ideas just enable things to organically find balance, or die. You don't
need any central control and in the worst case some part of the system will suffer a lot and will require
some attention, but no police controlling/overseeing everything was required, ignoring reliability
issues on a team has a direct impact/feedback on their day to day work, and trying to get around it
on the long run will also hurt the team, there is no place to hide (like some process or organization).

There is actually another rule that is also pretty cool and finds a lot of resistance:

* SRE's rotation

This basically states that every developers must do SRE work eventually. On the presentation it is
said that ALL teams resist this idea, with a miriad of reasons =P. Yet, it must be done as a rule, since
it is essential to people understand what is involved on reliability and operations. The bases for this rule
is a human cognition fact that "what I see is all there is", people that never see operational and reliability
issues will never be aware that they exist, just talking about it will not work because it is not how
the human brain works, that simple.

There is another important aspect of the error budget idea, the budget is for product/service that is being
monitored, not by teams.
This is important to achieve self policy when there is more than one team involved on the same service/product.
Lets way team X and team Y are working together on the same product. Team X does a lot of automation work and
delivers reliable code while Team Y releases unreliable code. When the error budget for the service is exceeded
team X will be unable to release code, even though they are doing a great job. This seems unfair, because well
it is unfare, this is good because people are pretty good to fight for fareness, no one likes to be screwed
over, so this will generate a huge amount of energy pressuring Team Y to do a better job from Team X. They even
may come up with some mandatory code reviews, or some other rules, but on this case this is not the product
of some central enforcement police that is not aware of context, this is emerging naturally from both teams
directly involved on the problem. And sometimes a solution may even come up with no need for rules at all,
the important thing is that there will be motivation for one team to interact with the other one regarding
reliability issues. 

The cool thing about the whole SRE approach is that it has very few rules to impose on developers.
But the very few rules that are imposed are extremmely hard to impose because it requires buy in of the idea from the
entire company, one weak link (specially if this weak link has a lot of power inside the company) will
render all the effort almost useless and you fallback to the mode where some teams are doing a great job and
other are doing a terrible job and no significative feedback is given to them and the problems they
generate are just spilled up everywhere and someone else ends up cleaning the mess (while they seem awesome
since they delivered feature x "in time").