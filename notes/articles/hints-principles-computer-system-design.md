# Hints and Principles for Computer System Design

Notes from the [paper](https://arxiv.org/pdf/2011.02455v3.pdf).

## Specs

There is an abundance of awesome advice on how to specify computer systems,
it would be pointless to just summarize all here (also a great deal of it leans
towards math and I'm math dumb, so would not be able to give insight into it),
but there is a few things that gets extra attention.

On the hallmarks of good specs, comes simplicity and the importance of
good design, with the bonus round of a Dennis Ritchie quote:

```
A spec should be simple, it should be complete enough,and it should admit code that is small
and fast enough. Good specs are hard, because each spec is a small programming language with
its own types and built-in operations, and language design is hard. Also, the spec mustn’t promise
more than the code can deliver—not the best possible code, but the code you can actually write.

In Dennis Ritchie’s words: “In spite of these limitations, the stream I/O system works well. Its aim
was to improve design rather than to add features, in the belief that with proper design, the features
come cheaply. This approach is arduous, but continues to succeed.”
```

With also the great insight that any problem that includes creating language will be
a very hard problem to solve, designing languages is hard work, they are basically
meta-thinking, you need to think about how you think about something.

One cool personal example that he talks about, on how hard it is
to write specs, is this:

```
In the late 1990s Paul England and I at Microsoft invented the mechanisms that
ended up in the Trusted Platform Module (TPM). By 2002 the TPM had been
standardized, and later I wanted to know some details about
how it worked. So I downloaded the specR107 and started to read it, but I got
stuck right away. Even though I actually invented these mechanisms, I couldn’t
understand the language of the standard.
```

Even the guy that dedicates so much effort and attention to specs, still fails
on creating language that he himself can understand later =P.

Now on the side of the bad specs we have remote objects and how they were
specified in the past. I remember reading something about the main reason
being that the error handling and the performance is way too different from a local
object in order for it to be successfully abstracted as a local object. And on this
paper I got extra confirmation for this, on leaky/bad specs Lampson states:

```
Errors or failures in the code mean that the code won’t satisfy the spec, unless the spec gives
it a way to report them. A common example is a synchronous API that makes the code look
local, fast and reliable even though it’s really remote, slow and flaky.
```

I also like how he models performance/reliability as part of the spec, decoupled
from code. I always see this completely unspecified, or done in an ad-hoc manner
on monitoring systems, usually as an after thought. Some notes on this, regarding
overloading (and probably admission control):

```
Similarly, contention or overload may keep the code from meeting the spec
if there’s no way to report these problems or set priorities.
```

He is actually pretty holistic on what emcompases the specification of
your software, and I tend to agree that working at that level produces
a more well thought system:

```
The spec describes all the possible visible behaviors of the system in terms
of the visible abstract state (which includes the arguments and results
of operations that the client invokes).

Sometimes people talk about a “functional spec”, a vague notion based on
the wrong idea that the system has no internal state or actions,
and that there’s nothing to say about its behavior beyond the results
that its actions return.

But reliability, failures, resource consumption, latency and system
configuration can all be visible state that a spec can describe.
```

On some domains some of this aspects are more explicitly part of the
spec, for example take latency, if the brakes of your care don't have
a clear spec of how much latency it may take to calculate something
you may end up dead with breaks that take seconds to respond =P.
So being precise on latency specification is fundamental on this case.

I also found interesting how he includes configuration as visible state
that needs to be specified, that also makes a whole lot of sense, people
tend to treat configuration as a second class citizen, "it is just a configuration
change", and when you see it is a complete meltdown on the entire system
"just because of a configuration".

One example from [Google](https://status.cloud.google.com/incident/cloud-networking/19009):

```
Two normally-benign misconfigurations, and a specific software bug,
combined to initiate the outage
```

As usual, reality is pretty complex and bad shit happens because 
of multiple different things going wrong at once, the perfect storm, but
it was also related to misconfigurations, maybe the idea of "benign"
misconfiguration should not exist, it is also a bug, a software bug,
given a well defined specification.

## Abstraction: Modules and Interfaces

On abstractions it is defined that a module is something with a clear
interface that can be composed in unforeseen ways, so it could be
an object, or not. So discussions around objects are just a subset
of abstraction.

### Objects

#### Inheritance

When talking about objects, as a subset of the topic of modularization, the
author is very spot on about some of the main dangers of classic inheritance
with method overriding:

```
The subclass provides code for the added methods, and it can replace or
override the superclass methods as well. The subclass should satisfy
the classpec of the superclass, which is all that its clients can count on.

Then there’ll be no surprises when a subclass object is used in code that
expects the superclass. An easy way to ensure this is to
not override the superclass methods, and to keep the added methods from
changing the private data of the superclass.

The final modifier in Java enforces this, but inheritance in general does
not. It’s very easy to break the superclass abstraction, because

− usually the spec is very incomplete and
− actually proving correctness is beyond the state of the art, so
− at most you get a guarantee that the method names and types agree.
```

It does reminds me of the rationale on how Go approaches object orientation,
you have interfaces, where you still can interpret the spec wrong (specifying
interfaces rarely is done just with the formal definition), but at least
you don't actually override a method implicitly with complete different
behavior, interfaces are pure spec, not spec + code, it seems the surface
for confusion/problems is considerably smaller.

Then the approach for extending types is composition, it introduces some
syntactic sugar but still there is no automatic overriding of methods, 
and it is impossible to access private data from the object you
are composing with, unless you reside on the same package as the object,
so it basically avoids the main pitfalls of inheritance, crazy overriding 
and change of internal state of the superclass.

I suppose it is good that Go "is not object oriented" =P.


A little more on abstraction/overloading:

```
There are two distinct ideas here: hiding (abstraction) and overloading
(using the same name for more than one thing).

• The class is doing the usual job of abstraction, describing the object’s
behavior and hiding its code from clients.

• The class is providing overloading for its methods, making it easy to
invoke them using names local to the class, but the same as the names of
methods in other classes with the same spec or a closely related one.

These two things work together when the different overloaded methods
do indeed satisfy the same spec, but can be a rich source of errors
when they don’t, since there’s no way to get the machine
to tell you.

This may be okay when the same team owns both superclass and subclass,
but it’s very dangerous if the superclass has many independent clients,
since there’s no single party ensuring that the subclass satisfies its spec
```

Basically overloading/overriding doesn't scale well, if you have control of the
whole software, like a single team, it can work well, but for anarchical
composition, like an OSS library used in wild ways by a lot of people, it
will probably not compose well. Maybe that is why usually frameworks get
traction, they don't allow wild composition, are less flexible and enforce
you to work with its own abstractions, which makes sense in classical object
oriented languages, where inheriting things and making mixin's (as in Python)
will probably go wrong with lots of different clients using the software for
different purposes.

### Components

Components are defined as this:

```
A module that is engineered to be reused in several
systems is called a component
```

Which is a tough topic, already starting with interesting quotes:

```
Reusing pieces of code is like picking off sentences from other people’s
stories and trying to make a magazine article. —Bob Frankston

It’s harder to read code than to write it. —Joel Spolsky
```

So even though re-implementing everything from scratch all the time
is unfeasible, just using whatever component you found on the internet
is also a recipe for disaster, re-usability feels to me like the holy grail
of computer science, people are always trying to achieve that
but it rarely works out well =P, at least in the classic sense where
someone is just implementing something and then has the idea to make
it generic/re-usable.

```
Obviously it’s better to find a component that does what you need than
to build it yourself (don’t reinvent the wheel), but there are some pitfalls:

− You need to understand its spec, including its performance.

− You need to be confident that its code actually satisfies the spec
and will be maintained.

− If it doesn’t quite do everything that you want,
you have to fill in the gaps.

− Your environment must satisfy the assumptions the component makes:
how it allocates resources, how it handles initialization, exceptions
and failures, how it’s configured and customized, and the interfaces
it depends on.
```

My overall feeling is that the industry focus way too much on
"don't reinvent the wheel" and almost nothing on assessing
dependencies (the wheel) properly. Personally I dislike the wheel
metaphor, what we do is extremely more complex than just
"if there is already a wheel just use it". Even wheels are not
that simple, like wheels for an airplane have a very different
spec from wheels on a car, and yet from the outside they look
the same, it seems to push people towards superficial analysis.

With so much software being open, intuition would dictate that
there would be a component out there for almost everything you need,
but in the end, there isn't, and with good reason:

```
Building a truly reusable component costs several times as much
as building a module that does a good job in one system, and usually
there’s no business model that can pay this cost.

So an advertised component probably won’t meet your needs for a reliable,
maintainable system, though it could still be fine if dependability is not
critical (for example, for approximate software).
```

This does reminds me of [Our Dependency Problem](https://research.swtch.com/deps),
it suggests an algebraic model to assess dependencies that is the product
of "chance of bad outcome (overall quality/robustness)" *
"impact when bad outcome happens", so when impact of failure is small,
assessing quality is not as important. And for each
dependency you have the sum of these products. Not a complete model, but it 
is simple enough to grasp easily and it has some truth to it.

How to avoid the dependency pitfall ?

```
There are two ways to keep from falling into one of these pitfalls:

• Copy and paste the module’s code into your system and make whatever changes
you find necessary. This is usually the right thing to do for a small component,
because it avoids the problems listed above.
The drawback is that it’s hard to keep up with bug fixes or improvements.

• Stick to the very large components usually called platforms. There will
only be a few of them to learn about, they encapsulate a lot of hard
engineering work, and they stay around for a long time because they have
a viable business model (since it’s impractical to write your own
database or browser). A well-maintained library can also be a source of
safe components that are smaller than a whole platform
```

So essentially, for core/big stuff find something solid, and for small features
it is preferable to do it yourself or even copy code. It is not the first
time I see advice like this, from [Go Proverbs](https://go-proverbs.github.io/):

```
A little copying is better than a little dependency.
```

Some people (including me) come from indoctrination on DRY, like any
repetition/copying is bad, as usual being dogmatic/idealist with stuff
doesn't go well in real life, so keep things DRY when it makes sense, and
replicate when it doesn't (life scales through replication, for example, it
repeats it self a lot =P).

# STEADY

On overall goals for software he proposes the cute acronym **STEADY**:

* Simple
* Timely (time to market, being the first to deliver)
* Efficient
* Adaptable
* Dependable
* Yummy =P (clients like the system)

It does capture a lot of the core goals with software design and it makes it easier
to discuss some of the tradeoffs that you have to make, like having something
less efficient and dependedable so you can have something timely/adaptable/yummy.

It is not impossible to have all, but as it is pointed out on the article, it costs
much more and most business can't afford that, so you need to be ready for some
unconfortable tradeoffs.

## Simple

Most of the advice on simplicity is very sound specialy because things are
presented as tradeoffs, opposing forces taht you should never just chose one
and stick with it forever, but I just loved the quotes used on the beginning,
so I feel compeled to quote the quotes =P.

```
I’m sorry I wrote you such a long letter; I didn’t have time to write a short one. —Blaise Pascal

Everything should be made as simple as it can be, but not simpler. —Albert Einstein

Simple things should be simple, complex things should be possible. —Alan Kay

The onion principle: doing a simple task is simple, and if it’s less simple, you peel one layer off
the onion. The more layers you peel off, the more you cry. (paraphrased) —Bjarne Stroustrup

Perfection is attained not when there is no longer anything to add, but when there is no longer
anything to take away. —Antoine de Saint-Exupéry

Beauty is our business. —Edsger Dijkstra

Beauty is more important in computing than anywhere else in technology … because software is
so complicated…. Beauty is the ultimate defense against complexity. —David Gelernter
```

Going back to simplicity, some of the advice reminds me why I usually don't like
frameworks/power tools:

```
A successful key module will grow over time, improving performance
with better algorithms and adding a few features, but building on a solid foundation. Make it fast
rather than general or powerful, because then the client can program the function it wants. Slow,
powerful operations force the client who doesn’t want the power to pay more for the basic function.
Usually it turns out that the powerful operation is not the right one. Well-known examples are
CISC vs. RISC instruction sets and guaranteed vs. best-efforts packet delivery.
```

And then a truth that I have observed happening over and over again:

```
Often a module that succeeds in doing one thing well becomes more elaborate and does several
things. This is okay, as long as it continues to do its original job well. If you extend it too much,
though, you’ll end up with a mess. Only good judgment can protect you from this.
```

Good judgement is hard to develop, it can't be teached (you can be a good example, but the
observer will need time to develop it) and this force towards making something sucessful bigger
seems universal, part of human nature.

## Timely

This means reaching out to the market at the right time, that doesn't mean necessarily
selling something, one example would be linux, in the end you have a user base and you
need to reach them on the right time,on the right way, or even if you have a much better
design you may just fail anyway (if success means adoption, there are other ways to measure
success, but for this discussion it seems that adoption is important).

This is very tricky, because on the realm of tradeoffs not everything is easier to add later:

```
Building a timely system (one that ships soon enough to meet your time-to-market needs) means
making painful choices to give up features and dependability. If it’s extensible you can add features
later; adding dependability is harder. It’s easier to make approximate software timely.
```

One concrete example is testing, which is related to dependability, ignoring it completely on the
beginning and then adding later is quite hard, much more hard than starting with the tests IMHO,
and yet sometimes you may have to make that decision, as far as people deciding are aware of this
it could be a good decision, the problem is being oblivious to the tradeoff.

## Efficient

On efficiency it defines it as:

```
Efficiency is about doing things fast and cheaply
```

And it is approached on the classic manner, if you are not sure you need it, don't do it,
specially because today other goals are usually more important than efficiency:

```
It used to be that machines were small and slow, and it was a struggle to get your problem to fit.
Today machines are big and fast, and for many problems efficiency is not an issue; it’s much more
important to be timely, dependable and yummy
```

But there are still some niches where efficiency is paramount:

```
But there are still plenty of
big problems: genomics, molecular dynamics, web search, social media graphs. And there are
devices with limited energy that can’t afford to execute too many instructions, and new real-time
problems where the computer needs to keep up with the human or with the physical world,
responding in just a few milliseconds
```

Overall it is not about not caring for efficiency, but efficiency is hard to attain,
anything that is hard should be done only if absolutely necessary, unless you are just
having an intellectual exercise:

```
It’s tricky to write an efficient program, so don’t do it unless you really need the performance.
If a shell script is fast enough to solve your problem, by all means use a shell script.
```

And if you optimze, there is the timeless advice:

```
first design, then code and debug, then measure, finally (if ever) optimize.
```

It doesn't make sense to optimize something that doesn't even work yet, and if
it works you need to measure so you can be sure it is slow and where.

It is not like the author doesn't care about performance, actually he pushes for
performance being part of the initial spec, something that you care and specify
before coding, so it is not about being all high-level/Java like and not caring
about anything until it works:

```
A system design needs to consider efficiency as well as simplicity and functionality, even
though it shouldn’t involve detailed optimization. To evaluate a design idea, start by working out
roughly how much latency, bandwidth and storage it consumes to deliver the performance you
need. Then ask whether with optimistic assumptions (including plausible optimizations), you can
afford that much. If not, that idea is no good; if so, go on to a more detailed analysis of the possible
bottlenecks, and of how sensitive the cost is to the parameters of the platform and workload.
```

The idea is that maybe the first naive implementation will be enough, given the spec, then you are good.
There is also some good advice of measuring things in a live system:

```
Your guess about where the time is going is probably wrong. Measure before you optimize. If you
depend on something unpredictable, measure it in the running system and either adapt to it, or at
least report unexpected values so that developers or operations staff can tell what’s going on.
```
