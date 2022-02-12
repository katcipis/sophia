# The Death And Life Of Great American Cities

Notes from [The Death and Life of Great American Cities](https://www.amazon.com/Death-Life-Great-American-Cities/dp/067974195X).

# Introduction

The book starts introducing the notion cities/urban planning is similar
as an ecosystem, where interactions/protocols between components of the
ecosystem are just as important as "things", like buildings and parks.

She describes this as an continuous process, with no beginning or end.
This reminds me a lot of "systems thinking" and Alan Kay original idea
for object orientation, aimed at interactions/protocols as the foundation
for composability and building more complex systems. It makes sense that the
disciplines overlap, urban planning for big cities is just as new as computing
and the challenges are also imense since the scale of the system is quite big.

Given the high complexity and the overall challenge of designing cities, the
book starts with a critique to the over simplistic and utopian way cities
were being planned at the time. Instead of keeping an open mind and getting
feedback from the system itself to learn what works and what doesn't the 
planners created an Ivory Tower from where they could do design given a whole
mental model of how people behave and how cities work that sounded really nice
but was completely disconnected from reality. That does ring a bell with overall
Software Architecture, people do that in a vacuum and just to satisfy their
egos, so it seems this is not a new problem just on the software industry.

# Expert Group Thinking

The same export group thinking phenomenon that I observe on the software
industry seems to have happened at the time for city planning, at least from
what the author describes. It is the same pattern of behavior.

People with power, armed with ignorance and hubris, keep pushing their agendas
on what "modern computing" is. This is done in an absolutist way since they
are "experts" and "thought leaders", even though their advice is completely
disconnected from reality and devoided from context.

When things go wrong the experts are so arrogant and disconnected from real
feedback (the classic echo chamber) that they are unable to learn from how
the actual system is behaving in a meaningful way, if it doesn't fit their
mental model of reality, feedback is discarded.

This is exactly what had seen to be happening on the realm of city planning.
People making decisions to how entire neighborhoods should live never even
set foot on the neighborhood and had no idea how it worked or what people
felt about living there. They had this utopian Radiant City/Garden City ideals
that they kept pursuing ignoring any feedback that maybe, that just didn't work
in reality.

Much of this was caused by lack of vision and oversimplifications, since actually
understanding such a big system as a city is extremely hard. One interesting
quote from the book:

```
It may be that we have become so feckless as a people that we no longer care
how things work, but only what kind of quick, easy outer impression they give.
```

This struck me hard since it expresses my exact feelings with software engineering,
and yet it was written on 1961 as a critique to how city/urban planning was being
done on the US. It made me feel less alone and also made me realize even more how
the software industry is not that unique in it ailments. The same cycles of expert
group thinking seem to have caused a lot of damage on city planning too. One depressing
thought is that it doesn't seem like we improved much since. But it is expected, in
a field that doesn't study even its own past, studying the past of others is ludicrous,
and if you don't know history you are bound to repeat the same mistakes.

My overall feeling about this is of great disrespect for the complexity of what
is trying to be accomplished. Most people proposing silver bullets that will make
everything simpler/safer/better demonstrate an enormous disrespect/ignorance towards
the actual complexity of software systems, just as the city planners ignored the complexities
of system life. Good ideas usually approach the problem with humbleness, realizing
there is no universal right answer and no utopia to be reached. But most ideas are bad,
and usually approaching the problem with naiveness/disrespect.

One quick example that comes to my mind was all the hype around NodeJS, when it was
proposed as an obvious awesome solution to concurrency issues (among other stupid things),
not having a concurrency model at all, just use callbacks and it will be simple/efficient
and awesome.

I got on that bandwagon from my sheer ignorance and trust on the industry,
needless to say that it didn't work well for me...
Never again (good read about this [here](https://joearms.github.io/published/2013-04-02-Red-and-Green-Callbacks.html)).

In the end the author compares the practices on city planning at the time with
[bloodletting](https://en.wikipedia.org/wiki/Bloodletting), it seems extreme but the
pattern in behavior is actually quite similar, you create a mental model of reality
that is completely off and you start treating it as a concrete reality, an universal
truth, so it doesn't matter how much it doesn't work or doesn't make any sense, you
keep doing it.

I find useful to get this lessons from other fields. All kinds of crazy ideas,
like bloodletting, once were accept as "the way" to do things by the group of experts,
and questioning them was regarded as stupid/arrogant. Since we are still humans, pretty
much the same keeps happening on the software industry centuries later :-).


# The Big Refactoring

Another striking similarity between city planning and software systems is
how big refactorings/redesigns are usually performed. A new group of software
engineers have a really big/awesome idea on how everything should be done,
including how everything on the current/old system is just wrong/stupid.

Then they start The Big Refactoring/Rewrite, having 0 consideration for
people who work/live on the current system, what works, what doesn't, etc.
There is no time to listen to what people think, you already have your awesome
idea, that given your synthetic mental model of how things work makes perfect
sense, that is it, it is perfect, it is awesome, and you just keep bulldozing
everything and everyone towards your awesome design.

I know this happens because I did this, and I saw other people doing it, and
guess what ? City planners were also doing, sometimes destroying perfect good
neighborhoods just because it didn't adequate to their mental model of "good
neighborhood".

Quote from the book on this:

```
Nobody cared what we wanted when they build this place. They threw our houses
down and pushed us here and pushed our friends somewhere else. We don't have a
place around here to get a cup of coffee or a newspaper even, or borrow fifty cents.

Nobody cared what we need. But the big men come and look at that grass and say
'Isn't it wonderful! Now the poor have everything!'
```

This reminds me a lot of our industry movements towards NoSQL that always results
on some sort of SQL layer on top, because for people that do advanced stuff
with data the relational model is just great, and yet developers that don't get it
keep pushing the data people towards "better"/"more scalable" models, because SQL
is ugly. I actually read a "thought leader" and owner of a "successful" company talking
about how software engineers should never write SQL, that it is a low level/error prone
thing to do manually.

SQL is actually a declarative high level language, more high
level than any general purpose language, how writing on it should be considered some
sort of useless/manual labor ? It is the same as advocating that all DSLs are terrible,
you should just always use objects because objects are great. Honestly it is beyond
stupid, and yet some degree of success on a specific niche makes people believe the
advice is sound, coming from an expert.

This also reminds me of Jamie Zawinski interview on Coders at Work. It shows perfectly
how people can ignore others + ignore their own failures and keep believing their
mental models about things are perfect and sound (it is always someone/something else's
fault). He was talking about the downfall of Netscape. A few quotes:

```
They thought just by virtue of being here, they were bound for glory
doing it their way. But when they were doing it their way, at their
company, they failed. So when the people who had been successful
said to them, “Look, really, don’t use C++; don’t use threads,” they
said, “What are you talking about? You don’t know anything.”

Well, it was decisions like not using C++ and not using threads that
made us ship the product on time.
```

```
There was just this big blank rectangle in the middle of the window where
we could only display plain text. They were being extremely academic
about their project. They were trying to approach it from the
DOM/DTD side of things. “Oh, well, what we need to do is add
another abstraction layer here and have a delegate for this delegate
for this delegate. And eventually a character will show up on the
screen.”
```

Then, about the same experience Brendan Eich also shares some of what happened
(both worked together at Netscape):

```
I have this big allergy to ivory-tower design and design patterns. Peter
Norvig, when he was at Harlequin, he did this paper about how design
patterns are really just flaws in your programming language. Get a better
programming language. He’s absolutely right. Worshipping patterns and
thinking about, “Oh, I’ll use the X pattern.”
```

```
There was an imperative from Netscape to make the acquisition that
waved the Design Patterns book around feel like they were winners by using
their new rendering engine, which was like My First Object-Oriented
Rendering Engine. From a high level it sounded good; it used C++ and
design patterns. But it had a lot of problems.
```

Interesting enough, a lot of what was happening on city planning reminds
me of all this. People find a bible and they go on evangelizing that and
pushing it everywhere, it is basic human behavior. Terrible for science and
engineering, but we are humans in the end, not scientists and engineers.


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

On the other hand, people gathered in concentrations of city size and
density can be considered a positive good, in the faith that they are
desirable because they are the source of immense vitality,
and because they do represent, in small geographic compass, a great
and exuberant richness of differences and possibilities,
many of these differences unique and unpredictable and all the
more valuable because they are.

Given this point of view, it follows that the presence of great numbers of
people gathered together in cities should not only be frankly accepted as
a physical fact.

It follows that they should also be enjoyed as an asset and their presence
celebrated: by raising their concentrations where it is needful for
flourishing city life, and beyond that by aiming for a visibly
lively public street life and for accommodating and encouraging,
economically and visually, as much variety as possible.
```

The overall pattern in behavior is to desqualify a possible solution
to a problem for emotional reasons. There was some high
density areas in big cities in the past that didn't performed well and 
truly were slums, but that was caused mostly because of poverty, bad
work law, lack of sanitation/technology. That created a very bleak
picture where lots of people living together is messy/unconfortable/unsanitary
while ignoring all the rich possibilities that comes from
having so much different people living together and using the city
in different ways.

Tools like proper sanitation and epidemiology enabled
high density areas to be safer and nicer, allowing us to get more of
their benefits without having to pay a hefty price for it, and yet the
benefits were not even considered by the people who were already convinced
that for sure it was a bad idea (high density = slum/bad).

Since this is related to how humans behave it also happens on software
development and the clearest example in my mind is, again, microservices.
Colored by bad examples, taken out of context, people created this
system of thought where "monolitic" applications are always bad, slums,
messy, ugly, won't scale, bleh. And the counterpart, a set of small services,
is always nice, well coordinated, clean, scalable and easy to maintain.

It doesn't matter how much some microservices architecture fail, or how much
some monoliths succeed, the emotional response is too strong, big is bad,
monolitic is bad, there is nothing to argue, nothing to see or learn from
the monolitic service that works well.

It struck me how the pattern in behavior is the same, and also how the people
affected on the urban plannig side of things were "experts" with decades
of experience.

Again an independent mind and critical thinking comes to the rescue,
avoid the pitfalls of "expert" group thinking and be aware of how
your emotions may be shaping your systems of thought.

# Understanding the nature of a problem

In the last chapter of the book there is an appeal to understanding the
nature of the urban/city planning problem before coming up with solutions.
Because if the you don't get the nature of the problem correctly there is
a good chance the solutions are going to be wrong. This feels simple and
intuitive, and yet it is very hard to do, specially for hard problems,
humans tend to interpret problems in the way that is more comfortable
to them, we are always working on representations on our minds, and the
representations can be very far off, colored by a multitude of biases
that are part of our nature.

A few examples of misguided representations on city planning where the ideas
around the Garden City and Radiant City, the Garden City was misguided in how
it romanticized the past and nature, and the Radiant City was misguided in
how having open areas around enormous buildings would be enough to make life
attractive.

Both ideas, which were widely adopted and defended by most of the Universities
and experts at the time, were off because of a misinterpretation of the nature
of the problem with cities.

The author uses a broad classification of problems, coming from mental models
for scientific thought:

* Problems of Simplicity
* Problems of Disorganized Complexity
* Problems of Organized Complexity

Problems of simplicity are defined as problems with two or three variables where
the variables are correlated in obvious ways.

Problems of disorganized complexity are problems with a multitude of variables
and where the variables are not correlated at all, it is used a lot in Physics
and the main approach to handling this kind of problem is statistics.

Problems of organized complexity are problems usually found on the life sciences,
like medicine, etc. You have a multitude of variables, but most of the behavior is
not random, the variables are correlated and you have intricate processes continuously
happening. In these kind of problems understanding how things integrate, communicate,
collaborate, affect each other, is fundamental. The mental model steers towards
processes, instead of thinking on objects/things.

The immediate connection of this with software is remembering Alan Kay, since
he always pushed for computing models that emphasize message passing/communication
and protocols, what is in between, a reasoning around processes and collaboration,
symbiosis and composition. He pushes for drawing inspiration from biology on how
to design systems, and this whole idea explained on the book kinda reinforced this
for me, software design is a problem of organized complexity, it can have some
randomness (like mutations on life), but it is very far from being a disorganized
complexity problem, and it is definitely not a problem of simplicity with just
a few variables.
