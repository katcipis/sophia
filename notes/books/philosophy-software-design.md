# A Philosophy of Software Design

The book [A Philosophy of Software Design](https://www.amazon.com/Philosophy-Software-Design-John-Ousterhout/dp/1732102201)
starts really well, admitting that in the field of
software engineering/computer science very little is discussed
about software design, the core issues that it addresses.

There is a lot of talk about magic solutions, patterns, styles,
that are usually presented (or used) as silver bullets and
don't go deep in what is the reasoning behind modularization, it is
just a recipe for you to follow. Recipes and rules are great when
you are starting, continuing with them forever is a terrible idea,
and the industry seems stuck on this first level, processes and
silver bullet methods/patterns.

It is like the difference between telling someone what to do and
teaching them how to think, teaching how to think is much harder
but much more effective in the long run.

The author wants to propose that we start discussing software design
again, at its core, which in his opinion is problem decomposition. There
is no single class about problem decomposition on computer science
courses and that is the core of true
mindful modularization in software. What is the objective of
modularizing ? What would be a good criteria for it ?

One of the last great papers about this, which is also mentioned
by the book, is Parnas
[On the Criteria To Be Used in Decomposing Systems into Modules](https://web.archive.org/web/20120223013018/https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf). I read it a few years ago and I admit it is truly
one of the best materials on how to modularize software I ever read.
It is a honest comparison of different strategies, where the judge
of the quality of the design is change, the thing that always cripple
software projects, change. It has no cute religious nonsense like "clean",
stuff that will make you feel good about yourself. It is true engineering,
just an exposition of trade-offs, and we haven't progressed much on this
regard in the author's opinion, and I agree.

The idea of the book is to discuss software design at a high level, a conceptual
level, the level of ideas, which is great because it is like learning how to
think instead of learning something that you are going to just repeat
forever in different contexts (design patterns, MVC, etc). I think that
is why he talks about philosophy, it is about thinking on how you think,
finding new ways of thinking, it is impossible to really assess
software design without meta cognition, thinking about form is thinking
about how humans understand form, be it physical or abstract.

This reminds me of a quote from the article
[The Computer Scientist as Toolsmith](https://cs.unc.edu/xcms/wpfiles/toolsmith/The_Computer_Scientist_as_Toolsmith.pdf),
it defines us (computer scientists, programmers, etc) as "engineers of abstract objects".
We build things but they exist on the realm of abstraction, so is the
form of the built artifacts, this makes it specially hard
to design software, we (as an species) are not that great
with abstractions in general.

One more digression before going on in the books ideas. Specially
today people may feel that talking about philosophy is useless,
you need to just learn how to build stuff and actually do something,
like philosophy is just some useless mind exercise. I agree with
Paul Graham vision that philosophy should not be vague, philosophy should
be extremely practical, it is thinking about how you think, why you
think that way, in a way that can changes you, improve you, and others
around you. So in a sense it is one of the hardest and more
important exercises you can do.

## Creation

I resonate with his view that programming is one of the most
creative activity humans have been involved since ever. The computer
is a very expressive medium and there is very little physical limits
to building software, if you have creativity and you are able to
organize your thoughts, you can build software.

It is like the painter with a blank canvas, but the canvas is gigantic
and multi dimensional, so the space to explore is vast. This reminds me
of a quote from [Structure And Interpretation Of Computer Programs](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-3.html):

```
I think that it's extraordinarily important that we in computer science
keep fun in computing. When it started out, it was an awful lot of fun.

Of course, the paying customers got shafted every now and then, and after
a while we began to take their complaints seriously. We began to feel as
if we really were responsible for the successful, error-free perfect
use of these machines.

I don't think we are. I think we're responsible for stretching them,
setting them off in new directions, and keeping fun in the house.
I hope the field of computer science never loses its sense of fun.

Above all, I hope we don't become missionaries. Don't feel as if you're
Bible salesmen. The world has too many of those already.
What you know about computing other people will learn.
Don't feel as if the key to successful computing is only in your hands.
What's in your hands, I think and hope, is intelligence: the ability to
see the machine as more than when you were first led up to it,
that you can make it more.
```

We have an amazing medium in front of us, using it to find "right answers"
that makes building products fast feels mediocre and depressing, at least for me,
it is the opposite of innovation (although the industry only talks about innovation,
it does not feel very innovative almost all the time). And ironically,
using these canned ideas sold by "Bible Salesman" makes build products
slower almost all the time, if the product has any real challenge on it,
so exploring the design space is not just about having fun, it is about
learning what is truly simple and minimal.

## Debate

The idea of the author is to start a debate, he is explicit in saying
that some of his ideas may be proven completely wrong in time and
he has not set out to define "best practices" or stuff that will
always be right, there is no substitute to thinking (beware of industry
methods and processes, thinking is expensive =P).

And he really seems to mean it, because almost every section, that has
some idea about design, also has a final section "Taking it too far",
which is a great idea, because the last time someone wrote about
design patterns (although this book is NOT about that) shit went
severely wrong (people took it as far as possible =P).

All the ideas he present he used on his own projects, and he seems to
have done some interesting work, from operational systems, file system
implementations, etc. He also applied them and discussed them with his
students. The book is actually a collection of him trying to teach
these core design ideas, which is a very good flow to refining ideas.
Which brings to the next topic.

## Teaching Good Design

It is clear that the difference of an average programmer and
a good one is the ability to do good design. He talks about almost
every good programmer he knows can't explain how they think, or
even explain what would be the principles and ideas of good design,
they just seem to do it, like it is an innate talent.

He doesn't believe on that, neither do I, and he mentions the same
book I read a few year ago,
[Talent is overrated](https://www.amazon.com/Talent-Overrated-Separates-World-Class-Performers/dp/1591842948).

Just because it is hard to explain and to learn, it does not mean
it is some sort of magic talent given by god. All good programmers/designers
have something in common, they built a lot of stuff along a lot of years,
and they got better incrementally. No one starts to program and is already
good at design as if god just implanted knowledge on his/her brain.

So he wanted to capture this greatness and teach it, it is a bold objective,
not even sure if it is actually possible, because it is hard to explain
anything related to "how" we think, but at least someone is trying.
And it truly seems to be in a way that helps people think about design,
not provide canned solutions that you can apply brainlessly because someone
else did the thinking for you.

## Why Good Design ?

It is very easy when you are new to programming to talk a lot about
design ideas, and the importance of design, without reflecting on
the actual need for design. Why we need good design ? It took me a lot
of time and reading
[The Humble Programmer](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html)
to understand that it is about human limitations. If programmers studied human
cognition and psychology, we would be much better on our jobs. In this sense
what we do is considerably different than other engineering's, in which way ?

Not that other engineers can ignore how humans think, but the limits of other
engineering's rest on physical laws, constraints of the universe where we exist.
When designing system's on a computer, there are some limits, but the first
limit you are going to reach usually is human, you built something awesome but
no one else can understand it, extend it, possibly even you won't be able
in a few months. The computer would be able to do much more, but you can't
because you can't reason about the system anymore.

This is related to our poor ability to handle multiple concepts
at the same time, that is why we need to split systems into modules, it makes
no difference for the computer. Just think about how programming languages
have nothing to do with computers, computers don't even handle text very well,
it is all about human cognition and its limitations. What makes things hard for
us and what makes them easy.

The purpose of good design is to fight complexity, in the sense that
for humans to understand it is easier, and it is easy to extend. We are
going to always reach a limit on how complex a system can be while
we can still understand it, but with good design the system can be bigger,
because all components of it are simple, so you can solve more complex problems.

With bad design, even a small system can already reach the limit.
So in the end it is about being able to build
more sophisticated systems and solve harder problems, in a way that works
because it is so simple that as you add all the complexity it is still
manageable (
[The Humble Programmer](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html)
uses the "Intellectually Manageable" expression).

Design also serves performance, but from my experience, the problem is usually
the human bottleneck, not computers. Coincidentally, at least for me, truly simple
designs will also have better performance, since they don't add useless overhead
or layers and are easier to extend in ways that won't be damaging and slow.

## Incremental Design

You get good design incrementally, this is not a new idea, but it is another
consolidation of the idea that you can't find good design from the void, without
actually trying to build something and getting feedback from it. Some forms of engineering
rely heavily on upfront design, but as said before, programming is a very
different kind of engineering, dealing with abstractions, and much more flexible,
and applied to all sorts of different domains (and also a very new form engineering).

Sometimes upfront design can be beneficial to software in my opinion,
but it depends a lot of context, and I agree that the rule of thumb is that it
is incremental in nature, specially because software usually welcomes it,
if you start with something simple, evolving it should be possible and
not that hard, some other engineering disciplines do not have that luxury.
Even the cost of undoing changes is just losing the time spent on the changes,
like a full revert on a version control system, other engineering disciplines
don't have that easy way to undo changes, so they are forced to be less incremental.

If you can evolve design, it seems to always make sense, because with time you
just learn more stuff, both about programming in general an about the problem
you are solving, so every day that passes you get better at solving the problem,
so updating your design with that new knowledge makes sense.

Some other cool quote's for incremental design:

* "Good design is redesign" from [Taste for Makers](http://www.paulgraham.com/taste.html)
* "good fit cannot be directly defined or designed; it is the absence of misfit I achieved by iterative design." from [The Computer Scientist as Toolsmith](https://cs.unc.edu/xcms/wpfiles/toolsmith/The_Computer_Scientist_as_Toolsmith.pdf)


## Complexity

One of the most cool contributions the book made to me is a simple mental model
of what is complexity and how to model sources/causes of complexity. As all
mental models it is incomplete, but it is precise enough to be useful, specially
useful when you feel something is off but can't explain why.

It is very important to try to define complexity, even when it feels subjective
and impossible to do so, or else you end up with vague advice like
"Keep It Simple Stupid", which is great advice and at the same time almost
useless given its lack of precision on what simple would be.
It is very easy to give correct advice if you are vague enough.

Making things "simple" seems to be useless advice because
no one starts with complexity as the goal, no one gets up on the morning
(or afternoon on my case) and says "today I'm going to create some complex shit".

Even the worse sins against simplicity were committed by people trying to
achieve it, people are usually looking for what simple would be, they just
have very different opinions of what "simple" would be, and the true answer
is really hard to obtain because it is not defined in terms of
engineering/computers, it is about people and psychology, so most programmers
are extremely badly equipped to generalize the concept of
simplicity/complexity (me included). But I digress, what would be the definition
and the models to reason about complexity ?

Complexity is anything that gets in the way
of understanding how a system works and how to extend and change it. This
seems like a good enough definition, because the holy grail of computer science
always has been how to come up with good abstractions that make it easier to
build/extend/integrate systems.

So given that complexity is what makes it hard to understand and change code the
next step is to define 3 common manifestations of complexity on systems:

* Cognitive Load
* Change Amplification
* Unknown Unknowns

Cognitive Load is the one that may feel more subjective, because it is related
to how much effort and time it is required to understand
a piece of code, and it is very easy to think something is simple, because you
can get it reasonably fast, but it actually isn't since other people have
extreme problems understanding it. It is also tricky because there is inherent
complexity and unnecessary complexity. High cognitive load is
necessary for hard problems, amazing designs makes these hard problems as simple
as possible, specially by the use of good abstractions and interfaces, but it
will still be somewhat complex/hard (because the problem is hard).
So the idea it to get rid of unnecessary cognitive load.

One example would be IO abstractions, if you need to open a file and read its
contents being obligated to understand how physical disks works is unnecessary
cognitive load, or details about how a specific disk driver works. It is cool
to open these abstractions to learn, but it should not be required if you
want to open and read a file.

Change amplification is very common on software and it is much easier to detect
and measure. Basically if every time you want to do just one simple change
on the software behavior, you need to change stuff on a lot of different places,
then you have change amplification and you need to do something about it soon.
There are some great examples of this on the
[On the Criteria To Be Used in Decomposing Systems into Modules](https://web.archive.org/web/20120223013018/https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf) paper. It revolves around the impact of change when comparing
different criteria, and usually the worse design is the one that changes
amplify greatly, like you choose a different database and now you need to
change all source files of your system, that is bad design and a lot of
change amplification.

Unknown unknowns would be when you think you understood how a system works, but
you didn't, usually because something is not explicit/obvious, and you end
up inserting a bug because you didn't know that you didn't know something =P.
This one is kinda funny but it is one of the most serious ones, since it is a
sure way to add bugs on a system.

Given the 3 manifestations of complexity 2 causes for them are presented:

* Dependencies
* Obscurity

I'm very fond to dependencies as a cause because I seen a lot of people
thinking their code is simple because it is only 3 lines of code, but
it uses a framework that is thousands of lines of code and introduces
a lot of unknown unknowns. This is by far the most tricky cause of
complexity and the one that is most ignored on "modern" software
development, most of the programmers I know seem to ignore
that dependencies add complexity to a system, it is just good sense
and "avoiding reinventing the wheel".

It is not just about third party dependencies, but also applies to
dependencies between modules that are part of the system itself,
when understanding one module requires deep understanding of 5 other
modules (the dependencies), this introduces cognitive load and possibly
change amplification. It is usually caused by a misguided notion that if
you have a lot of tiny modules than your system is simple, size as a criteria
for module decomposition (modules can be services, so yes I'm looking at
you micro-services =P).

Obscurity is the more traditional/obvious one, code that is very dense
and filled with hacks that are very unintuitive will be an obvious source
of unknown unknowns and bugs. Bad naming is the most classical source of
obscurity too, which adds to cognitive load (the name doesn't help and 
even mislead) and to unknown unknowns (you think you got it, but you didn't =P).

And that is pretty much it, it is not a complete view of complexity but it
is simple enough to be easy to remember and reason with, specially when
discussing design issues with other people, in a way that is not entirely
subjective.


## Strategical VS Tactical

The most important thing for achieving good design is the
right mindset, and very important part of that mindset is that
**working software is not enough**. That is the difference between
the tactical programmer and the strategical one.

The tactical programmer just want to find the fastest path to
delivering the next feature, or fixing the bug. He will not spend
extra time thinking on design issues, he optimizes for changing as
little as code as possible. This has tactical value, since less change
is less chance to break things, but also eliminates design improvements
as part of the development process, you just (almost) always makes
things a little worse, by adding a little more complexity, a little
new shortcut in the code.

It is like death by a thousand cuts, it never seems like a big deal
but when you sum up all the small additions you end up with a mess.
It is also psychologically hard to get out of this situation, because
every little improvement that you make now seems insignificant given
the terrible mess that is the code (and then the even usually worse
idea of rewriting from scratch).

Fixing bugs also provides valuable opportunities for design improvements,
because bugs are usually caused by bad design, which makes things confusing
and gives space for unknowns unknowns, but a tactical programmer won't take
advantage of that opportunity. There is a very interesting interview with
Rob Pike named
[The Best Programming Advice I Ever Got](https://www.informit.com/articles/article.aspx?p=1941206),
he was talking about how Ken Thompson taught him how to debug (by example):

```
After a while I noticed a pattern: Ken would often understand the problem
before I would, and would suddenly announce, "I know what's wrong."

He was usually correct. I realized that Ken was building a mental model
of the code and when something broke it was an error in the model.
By thinking about *how* that problem could happen, he'd intuit where the
model was wrong or where our code must not be satisfying the model.

Ken taught me that thinking before debugging is extremely important.
If you dive into the bug, you tend to fix the local issue in the code,
but if you think about the bug first, how the bug came to be, you often
find and correct a higher-level problem in the code that will improve the
design and prevent further bugs.

I recognize this is largely a matter of style. Some people insist on
line-by-line tool-driven debugging for everything.
But I now believe that thinking—without looking at the code—is the best
debugging tool of all, because it leads to better software.
```

Even though it touches the subject of using or not debugging tools, the
most important part is that bugs are usually faults on the model of
the software and that it is important to address the underlying design
issue instead of just fixing the bug tactically.

One interesting concept that he introduces is the "tactical tornado",
it is that programmer that management loves because he can 
deliver features extremely fast, like a tornado,
but just like a tornado he leaves complete destruction after he is finished,
a mess that will take months and sometimes years for other people to clean.
Every company has one, and one of the most important cultural changes a
company can make is to not reward that kind of behavior.

The strategic programmer on the other hand is the opposite, he wants
to deliver software as fast as possible, but not in detriment of the
future, so he takes time to address design issues and fix them, even if
it means more work now. He also spends more time experimenting with
different designs to solve a problem, so he can choose the design
that fits best the problem, because just making it work is not enough.


## Deep modules

Before going in this topic it is important to define what is
a module in this context. Here a module can be an object,
a package, or a service. It is a unit of code that is cohesive
and has a public interface, be it local or remote.

This is one of the most interesting chapters of the book since it
gives advice that is very sound and yet goes against the current
fad of micro-services. Instead of using being small as a way to
define that a module is good, it uses the idea of being deep, which
is the opposite.

In the opinion of the author, good modules are deep, in the sense that
they have small and simple interfaces (the interface is not very wide),
which hides a lot of complexity underneath (hence deep).

Usually breaking up a module just because
its implementation is big will only spread the complexity around,
which the maintainer will need to understand anyway, and introduce
more interfaces, so instead of having just one simple interface you
may end up having N interfaces to smaller implementation. Which is
the situation that gives birth to ideas like aggregation layers, 
because composing all this small interfaces is complex,
so you build the interface you wanted all the way along on top of it.

In the end you cause more trouble to the clients
of your module just to satisfy your desire for small modules. This
will also cause trouble to maintainers too, since you will end up
jumping back and forth through a lot of small/shallow modules to
understand even basic things about the system.

His examples of shallow modules are all related to Java, which is a nice
example of the mindset of "objects are good, so lets have a lot of them".
Even to do simple tasks you end up having to create multiple objects
and composing them, which adds complexity to clients. This made me
remember how I felt when I wrote a program to read a file contents in Python
and compared it with Java, it made me shift to Python for all my
work in college and I never looked back. So you have all this micro/small
everything thing that kinda makes sense, but the end result is just
terrible to work with (always composing one trillion small things).

In the case of micro-service this is even worse, because composing
and understanding multiple services is harder than local objects.
So the whole movement gives me bad feelings,
because using as a criteria for modularization "always be small"
does not seem like a good idea (nothing wrong with sometimes being
small). 

Good abstractions are about information hiding, that is what makes
an abstraction something simpler than the concrete thing, the more
information you can hide the more you gained, specially if you can
make it with a small and simple interface. This gives a very nice model
to evaluate the quality of a module, the smaller the interface with
more information hidden, the better.

It may feel funny to say that because it seems to favor big/complicated
implementation of small interfaces. This is not the idea,
the idea is that the more interfaces your clients need to know more
complexity has been added to the overall system. If you have a module
with a small interface, but that has very little information being hidden
on it (shallow) only two things can happen, either your problem is VERY
simple and that is enough (and the module seems kinda useless)
or you will need several other shallow modules,
each one with its simple interface, which when put together ends up
being complicated to clients (combining multiple interfaces that do
almost nothing). That gets even worse if the modules are services,
as in micro-services, because combining services is never very
easy (in my experience at least), specially when compared to combining
command line tools in a UNIX like shell, which was one of the inspirations
of the whole micro-services thing, one tool does one thing well,
but in a perverted way.

One concrete example given in the book about a deep module is
file I/O on UNIX, the interface to manipulate files on UNIX is
very simple and small, but it hides a LOT of complexity (block devices,
hardware drivers, etc). Another one is networking, a TCP connection
delivers to you a nice ordered stream of bytes as an abstraction but
hides a lot of complexity behind it (traffic congestion, handshake,
retransmission, etc).

Another example would be the grep command, it has a very small and
easy to use interface, it is easy to compose, but it is not that
small, at least not when compared to micro/nano services non-sense
that services needs to have just 100 lines of code etc,
it hides considerably complexity from the caller through a very
simple interface.

Of course that on the issue of information hiding you can also be
wrong on the direction of hiding necessary information, which
will also result in a bad design. The most classical example of
this being remote objects that tried to hide in the abstraction
failure modes that can't be safely hidden from the programmer.

In the end the best take away from the whole deep modules
idea is to consider the cost of interfaces and how
clients compose them when thinking about complexity instead of only seeing
the complexity of the implementation. When a service has a lot of complex
code but its interface is small and simple, just refactoring the service will
probably be better than splitting it up into multiple smaller ones.

## To split or to not split, that is the question

I think overall the industry focus way too much on when things
are too big and should be split, which is a perfectly valid (and common)
scenario. But you can also be wrong when you are splitting something
in two, when they actually belonged together, and he also talks about
this kind of mistake (which is usually related to shallow modules).

The usual signs that you separated something that should be together are:

* Lots of jumping around between two modules to understand just one concept
* Most of the abstractions are the same or related
* Strong conceptual overlap, most of the time they talk about the same stuff.
* Every time you need to change something on module A you also need to change on B
* Clients always use both modules together

If you just replace modules with service you get a nice way to check when
you separated something in multiple services, but you shouldn't. It will
feel like you have high coupling issues, but the truth is that you actually
lack cohesion, both modules should be together, a single unit, splitting them
created this high coupling scenario. So even high coupling may be a signal
that something should be together (not always of course).

On reasons to split, one thing that caught my attention that I really liked
is separating general purpose code from application specific code, that is
indeed a very good reason for separating code in different modules. The rest
reminds me a lot of single responsibility principle or Parnas advice
of isolating the system from tough design choices.

## Design errors out of existence


## Documentation as part of interface
