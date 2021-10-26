# The Death And Life Of Great American Cities

Notes from [The Death and Life of Great American Cities](https://www.amazon.com/Death-Life-Great-American-Cities/dp/067974195X).

# District Density And MicroServices

Besides acquiring some interesting insights on urban planning,
one of the most interesting aspects of reading this book
is finding some patterns on how people behave, specially group
thinking (or its even worse cousin, "expert" group thinking) and
on the topic of "ideal" district density another fallacy surfaced
that reminded me of a similar one done by the software industry,
trying to find ideal sizes/densities for things in an universal
way (basically, a context free easy answer),
that sounds great in theory but completely disconnected from reality.

For district density, the idea was having this theoretical value
that shows the ideal ratio of how much people should live per
acre of land that is always optimal, assuming that everytime too much
people live agglomerated (high density) that is bad (like a slum). 

For services/software, the idea there is a similar trend that it
should always be small (leaving the definition of small hanging),
because theoretically handling a bunch of small services/runtimes
is always best than a bigger one. It evoques a very similar emotional
response, a lot of code linked together is ugly, dense, "slum" like,
while small code on small runtimes is neat and precious, organized, elegant.

For software some people even try to come up with amount of lines of
code for defining what is "micro", which doesn't make much sense but
the terrible name given to the idea (**micro**services) does push you
towards thinking on literal size (how do I know if I'm doing **micro**).

Now some interesting ideas from the book, for population density on
a district the author defends that there is no golden ratio that
works in all situations, she converges to contingent/context aware
definitions instead of global/utopic truths.

```
What are proper densities for city dwellings?

The answer to this is something like the answer Lincoln
gave to the question:

"How long should a man's legs be?"

Long enough to reach the ground, Lincoln said.

Just so, proper city dwelling densities are a matter of
performance. They cannot be based on abstractions about
the quantities of land that ideally should be allotted for
so-and-so many people (living in some docile, imaginary
society).

Densities are too low, or too high, when they frustrate
city diversity instead of abetting it. This flaw in performance
is why they are too low or too high.

We ought to look at densities in much the same way as we look at
calories and vitamins. Right amounts are right amounts because
of how they perform. And what is right differs in specific instances.
```

On top of this idea that it always depends on your context she
provides examples where low density may work, where high density
may work, and also where both breaks. The core principle to keep
in mind is performance, you need a good way to measure how an
idea is performing, you need to understand the core properties
that you need to retain for something to work well, and on what
that depends.

In the case of urban planning she defends diversity of use as
one of the most important core properties,
you need a diverse set of people, doing different things, at
different times of day, to keep a lively neighborhood.

So the right density is the one that for the given location,
given other circumstances of the place, foster diversity of usage
(there are other properties, but I want to keep the notes brief =P).

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

From [Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/introduction.htm):

```
A good architecture is not created in a vacuum. All design decisions at the
architectural level should be made within the context of the functional,
behavioral, and social requirements of the system being designed, which is
a principle that applies equally to both software architecture and the
traditional field of building architecture
```

That sums up well the fallacy of default answers, like "microservices" or
do "pubsub" because you are basically architecting on the vacuum and it
also shows how hard the problem really is, you need to consider even the
**social** requirements of the system you are designing, who is going to
interact with it ? how ? (both clients and also engineers changing the system
behavior).

But since the whole point here is to make an analogy, what would be the
core principle that helps performance when maintaining distributed
software ? This is as tricky for software as it is for urban planning,
a lot of urban planning ideas will seem like a good idea for a few years
but then they collapse as time passes and create serious social issues, for software
it is very easy to measure performance literally, in terms of how fast
it is to build new features, fix bugs, response times, etc, and that is equally prone
to create a situation where you are really fast for some time, maybe even
years (but usually much less) and all of a sudden performance falls
dramatically (on all its dimensions) and you find yourself in a very bad place.

More than once I was in a situation where the whole point of a huge effort
was to fix this, so it is a very recurrent pattern on software development,
you think you are doing great and being fast and eventually everything crumbs
down and you are left with a pyramid of debris to deal with. When faced with
this, some people try to build something new from scratch, better architected
so it can stand on its own, some other people just convince themselves they
always wanted a pyramid =P.

We need to take into consideration a lot of other concerns that are
typically associated with proper software engineering, the topic of
how "easy" it is to understand a system and change its behavior.
All the issues that are introduced to software when you add time
and people to the equation.

This is a very important indicator of performance, this would be one of the
core principles that should drive the correct size of services and where
to put boundaries (not some universal/naive concept of "micro"). And yet
it is very elusive because it is very easy to fool yourself into thinking
you are building software that is easy to maintain/understand, for example,
a few indicators of complexity in a software system:

* Cognitive Load
* Change Amplification
* Unknown unknowns

These 3 are good examples of things that can make a system really hard
to maintain and evolve, but the only generator of complexity that is reasonably
easy to detect/measure is change amplification, the other two are very
tricky to detect you are falling into them (you can go full
[boiled frog](https://en.wikipedia.org/wiki/Boiling_frog) ).

I saw terrible code that people thought were great, meaning cognitive load was high
for me but they seemed OK with it, and on unknown things..well they are by definition
unknown...so how can you measure it ?

It is not entirely helpless, if you have good engineers on an organization, even
if it is hard to measure these kind of things in a discrete space, they will be
able to get a sense if something is in a good state or not and do something about it,
that is why we have metaphors like "code smell", you feel something is wrong
but it is hard to explain all the reasons for it.

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
* Orthogonal (relates to resiliency, easy to change)
* Easy to deploy/operate
* Observable

All that also counts as "overall performance" of the system, how well it is doing,
and it is fundamental when making decisions about boundaries and service size.
But just as in urban planning, on software the decision on boundaries/size is
actually really hard because you have this mesh of different properties that
you need to satisfy and some of them can even be contradictory to each other,
you push for one and lose the other.

On top of that you have context, the whole social context of where the software
is being built, by who, etc. Given all that complexity it is easy to create
simpler mental models and just create ideas that work on this simpler/ideal
mental model, but that has only the benefit of being more comfortable, it
doesn't produce great design. For great design there is no easy rule of thumb
thing to do all the time, like you learn once and do it forever and you are golden,
some things you should be doing all the time is:

* Thinking critically about everything
* Understanding your context
* Gather feedback on system performance
* Evaluate feedback honestly
* Be cynical about easy answers and "thought leaders"/"experts"

And by "thought leader"/"experts" I mean people that give non-contingent advice
about things, just do this, just do that, everything will be better, these
people are not truly experts (even if a industry says so).

There is also a great lesson on this book on challenging status quo and
systems of thought, most experts had amazing academic credentials and built a lot of
things that didn't worked as well as they could, but they created an echo
chamber of reinforcement that it was actually great and keep patting themselves
on the back. Any feedback on failed planning was always someone else's fault,
the urban planning itself was perfect, by the book.

Instead of being kept on this system of thought the author
challenged it by paying attention to actual running cities, what made
them work, what didn't, understanding the whole thing as process involving
complicated interactions, and once she was convinced most experts were
wrong she just kept pushing for it, instead of falling on the fallacy of
expert group thinking, doing what the current industry is doing.

Another lesson is that she did all that by focusing on people, how
they behave, what they like, what make them feel safe and engaged, not just
some theory deprived of the human component.


# Systems Of Thought

There is some interesting ideas around how our systems of thought
are always based, at least partially, on emotion, and how that
can lead to irrational/dettached from reality mental models.

```
Systems of thought, no matter how objective they may purport
to be, have underlying emotional bases and values.
```

No one is able to get rid entirely of emotion when building
their own systems of thought, it is just how humans are wired,
but it seems to be an advantage to at least be aware of how
colored by emotion we may be, so we can avoid the pitfall
of insisting on ideas that evidence suggest are bad.

And as you can image, this is exactly what happened on urban planning
around the time the book was written. There was a great aversion to
high density areas, with focus on open space and low density.
Given the empirical failure of most monotonous/open space/low density
neighborhoods it was expected that people would realize the mismatch
between reality and their systems of thought.

And yet there was insistence on the urban planning field that high
density was universally bad. Even if some low density places worked well
in the context of cities, which is rare, they would deny the success
of high density areas and insist on them being slums. Independent
of how well a neighborhood performed, if it is high density, it must
be a slum:

```
However, reason does not rule this world, and it will
not necessarily rule here. The unreasoning dogma that
healthy areas like the high-density North End of Boston
must be slums, or must be bad, because they are high-density,
would not have been accepted by modern planners as it has
if there were not two fundamentally different ways of looking at
the question of people in dense concentrations--and if those two
ways were not, at bottom, emotional.

People gathered in concentrations of big-city size and
density can be felt to be an automatic--if necessary evil.

This is a common assumption: that human beings are
charming in small numbers and obnoxious in large numbers.

Given this point of view, it follows that concentrations of
people should be physically minimized in every way.
```

The overall pattern in behavior is to desqualify a possible solution
to a problem for emotional reasons. There was some high
density areas in big cities in the past that didn't performed well and 
truly were slums, but that was caused mostly because of poverty, bad
work law, lack of sanitation/technology. That created a very colorful
picture that lots of people living together is messy/unconfortable/unsanitary
while ignoring all the rich possibilities that comes from the diversity of
having so much different people living together and using the city
in different ways.

Tools like proper sanitation and epidemiology enabled
high density areas to be safer and nicer, allowing us to get more of
their benefits without having to pay a hefty price for it, and yet the
benefits were not even considered by the people who were already convinced
that for sure it was a bad idea.

Since this is related to how humans behave it also happens on software
development and the clearest example in my mind is, again, microservices.
Colored by bad examples, taken out of context, people created this
system of thought where "monolitic" applications are always bad, slums,
messy, ugly, won't scale, bleh. And the counterpart, a set of small services,
is always nice, well coordinated, clean, scalable and easy to maintain.

It doesn't matter how much some microservices architecture fail, or how much some
monoliths succeed, the emotional response is too strong, big is bad,
monolitic is bad, there is nothing to argue, nothing to see or learn from
the monolitic service that works well.

It struck me how the pattern in behavior is the same, and also how the people
affected on the urban plannig side of things were "experts" with decades
of experience.

Again an independent mind and critical thinking comes to the rescue,
avoid the pitfalls of "expert" group thinking and be aware of how
your emotions may be shaping your systems of thought.
