# The Death And Life Of Great American Cities

TODO: Some intro + book link

# District Density And MicroServices

Besides acquiring some interesting insights on urban planning,
one of the most interesting aspects of reading a book like this one
is finding some patterns on how people behave, specially group
thinking (or its even worse cousin, expert group thinking) and
on the topic of "ideal" district density another fallacy surfaced
that reminded me of a similar one done by the software industry,
trying to find ideal sizes/densities for things in an universal
way, that sounds great in theory but completely disconnected from
reality.

For district density, the idea was having this theoretical value
that shows the ideal ratio of how much people should live per
acre that is always optimal, assuming that everytime too much
people live aglomerated (high density) that is bad (like a slum). 

For services/software, the idea that it should always be small
(leaving the definition of small hanging), because theoretically
handling a bunch of small services is always best than a bigger one.
For software some people even try to come up with amount of lines of
code for defining what is micro, which doesn't make much sense but
the terrible name given to the idea (micro) does push you towards
thinking on literal size.

Now some interesting ideas from the book, for population density on
a district the author defends that there is no golden ratio that
works in all situations, she converges to contingent definitions
instead of global/utopia truths.

```
TODO: cool quote from the book
```

On top of this idea that it always depends on your context she
provides examples where low density may work, where high density
may work, and also where both breaks. The core principle to keep
in mind is performance, you need a good way to measure how an
idea is performing, you need to understand the core properties
that you need to retain for something to work well. In the case
of urban planning she defends diversity as this core property,
you need a diverse set of people, doing different things, at
different times of day, to keep a lively neighborhood.

So the right density is the one that for the given location,
given other circumstances of the place, foster diversity of usage.
The whole point is that it is complex, usually diversity of usage
is influenced by a lot of things in a district/neighborhood, density
is just one of them, so it makes very little sense to try to find
a golden/ideal value that should be applied everywhere.

On the topic of software design the same thing happens, people tend
to search for easy answers on how to make things work, like you are in
a messy situation, how to get out of it ? Just make services smaller.

Problems usually go much deeper than just service size/boundaries,
and that is usually a reflection of other deeper social issues
on an organization, but addressing the problem holistically is very
hard, so people tend to give too much attention to non-contingent
expert advice on how to solve problems, it is comfortable and easier
but probably won't solve any of your real problems.

But since the whole point here is to make an analogy, what would be the
core principle that helps performance when maintaining distributed
software ? This is as tricky for software as it is for urban planning,
a lot of urban planning ideas will seem like a good idea for a few years
but then collapse in time and create serious social issues, for software
it is very easy to measure performance literally, in terms of how fast
it is to build new features, fix bugs, etc, and that is equally prone
to create a situation where you are really fast for some time, maybe even
years (but usually much less) and all of a sudden performance falls
dramatically and you find yourself in a very bad place.

More than once I was in a situation where the whole point of a huge effort
was to fix this, so it is a very recurrent pattern on software development.
We need to take into consideration a lot of other concerns that are
typically associated with proper software engineering, the topic of
how "easy" it is to understand a system and change its behavior.

This is a very important indicator of performance, this would be one of the
core principles that should drive the correct size of services and where
to put boundaries (not some universal/naive concept of "micro"). And yet
it is very elusive because it is very easy to fool yourself into thinking
you are building software that is easy to maintain/understand, for example,
a few indicators of complex in a software system:

* Cognitive Load
* Change Amplification
* Unknowns unknowns

These 3 are good examples of things that can make a system really hard
to maintain and evolve, but the only generator of complexity that is reasonably
easy to detect/measure is change amplification, all the other two are very
tricky to detect you are falling into them.

I saw terrible code that people thought were great, meaning cognitive load was high
for me but they seemed OK with it, and on unknown things..well they are by definition
unknown...so how can you measure it ?

It is not entirely helpless, if you have good engineers on an organization, even
if it is hard to measure deterministically these kind of things, they will be
able to get a sense if something is in a good state or not and do something
about it.

For example, unknown unknowns usually come from cryptic code, the
code is so odd on its form, so filled with implicit things, lacking clarity,
that even if you can't prove it is buggy, you have a deep sense of dread
and you know that something is lurking there.

Also I have seen people ignoring even the easy/measurable things, like
change amplification. One of the classic microservices mistake is building
the distributed monolith. The services are "isolated" at runtime/network level,
but a problem in a service breaks almost everything in very obscure ways and
adding a single/simple feature requires work to be done on multiple services
every time, this is a clear indicator of change amplification (1 change amplifies
into changes in 10 services for example), and yet people ignore this as normal
and being the price for microservices. So if you take at least this seriously
you are probably better off than most organizations.

And then it gets even more complex, besides the maintenance/extension side
of things your system also needs other properties, like:

* Scalable (to some desired point)
* Resilient
* Efficient
* Orthogonal (relates to resiliency)
* Easy to deploy/operate
* Observable

All that also counts as "overall performance" of the system, how well it is doing,
and it is fundamental when making decisions about boundaries and service size.
But just as in urban planning, on software the decision on boundaries/size is
actually really hard because you have this mesh of different properties that
you need to satisfy and some of them can even be contradictory to each other,
you push for one and loose the other.

On top of that you have context, the whole social context of where the software
is being built, by who, etc. Given all that complexity it is easy to create
simpler mental models and just create ideas that work on this simpler/ideal
mental model, but that has only the benefit of being more comfortable, it
doesn't produce great design. For great design there is no easy rule of thumb
thing to do all the time (like you learn once and do it forever),
the only thing you should be doing all the time is:

* Thinking critically about everything
* Understanding your context
* Gather feedback on system performance
* Evaluate feedback honestly
* Be cynical about easy answers and "thought leaders"/"experts"

And by "thought leader"/"experts" I mean people that give non-contingent advice
about things, just do this, just do that, everything will be better, these
people are not truly experts (even if a industry says so).
