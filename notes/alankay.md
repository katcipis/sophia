# Alan Kay Cool Stuff

Sources for these writings:

* [Power of Simplicity](https://www.youtube.com/watch?v=NdSD07U5uBs&feature=youtu.be)
* [The Computer Revolution not happened yet](https://www.youtube.com/watch?v=oKg1hTOQXoY&feature=youtu.be)
* [Rethinking CS education](https://www.youtube.com/watch?v=N9c7_8Gp7gI)
* [Normal Considered Harmful](https://www.youtube.com/watch?v=FvmTSpJU-Xc)
* [Doing with images make symbols](https://www.youtube.com/watch?v=p2LZLYcu_JY&feature=youtu.be)
* [ARPA / Xerox PARC culture](https://www.youtube.com/watch?v=wdHtYW_wcAs)
* [Keynote: Making Progress — Alan Kay](https://www.youtube.com/watch?v=9MqVfzxAp6A)

## Science definition

Greatest thought process ever created by humankind exactly
because it is not about "truth". There is reality and there is
our limited brains and how can they represent reality. Science is about
building N representations of the reality, all of them are false on some
sense since our brains are very limited, and realising that and exploring
this representation space is a very powerful concept.

To be able to see you have to realise you are actually blind.

## Mind as canyons

This metaphor compares our view of reality as canyons that are formed by erosion.
As time passes they only get deeper and it gets really hard to see out of them.

It is human nature to die in the same world that you was born, it has been like that
for thousands of years, so it is on our nature to resist change and also to get
attached to our representation of reality and ideas.

There is a great way to observe that on astronomy.

## Circles VS Ellipses

I won't recall the names, because I suck at remembering names =(, but
basically for hundreds of years man tried to explain the orbit of planets
using circles. Why ? Well, it seemed like the planets where moving on a very
messy way (the word planet in greek refers to "wanderer"), and God is perfect,
since God is perfect the way everything works must be perfect too.

So instead of accepting old works that suggested elliptic orbits, there was a lot
of work to explain it with circles. Since circles where not fitting the reality did
they revised the circle/perfection thing ? No, humans are not really great with this
kind of thing remember ? =) They just started to explain things with multiple circles,
basically patching their broken model with more circles.

Certainly this reminds me of something =), just as we patch bad ideas we patch
bad software, a huge failure of imagination but at the same time part of our nature,
and changing human nature is much slower than we would like to believe.

The end of the history is that the guy who was able to establish the elliptic orbits
model took decades of trying to work with other models. Why ? Well he thought that
ellipses where so basic and simple that the previous geniuses SURELY would have thought
of that and discarded because it did not worked. They where geniuses alright, but still
human, his lack of understanding human nature costed him some decades =).

This shows another side to studying and respecting previous work. The software industry
suffers a great deal for not knowing the past and repeating its mistakes, but sometimes
it is important to let go of old assumptions and try again an idea.

## First human powered flight

McReady was the first man to implement successfully human powered flight.
What is cool on the history is how he was able to do that, basically two things:

* He was not trying to build a plane, he was trying do to human powered flight
* He found a way to get feedback fast

The fast feedback is key, everyone expended months rebuilding the planes after a
failure. He built a VERY simple prototype, rebuilding and hacking it was pretty fast.

With this process of "try, fail, correct, try again" he was able to get it working in
six months (some of the planes took almost this just to be built).

## Burma monkeys

Monkeys at Burma are captured by putting a nut inside a jar. When the monkey
grabs the nut it can't take its hand from the jar. The monkeys could get free
at any time just by releasing the nut, almost none of them do this, they get
caught because of their inability of letting go. This is a human trait as well,
since we are primates :-).

Sometimes to get something you need to let it go.

## Science of Computing

There is a great deal of effort on presenting computing science as something
completely unrelated to computers, which makes sense, just as thinking architecture
has everything to do with autocad, it does not make sense.

The original idea of having a science for computing was like a science for
processes. The way we think is a process, an entire nation is a process,
in the way they interact and operate.
The idea was to study how to represent this kind of
phenomenons as processes and study them (not necessarily implementing them on
a computer, although this reminds me a lot of simulations).

He mention the American constitution as an amazing process that keeps the nation
working almost unaltered for more than a hundred years, for example.

This is very hard for me to understand since I'm pretty low level / machine
oriented on how I think about computing, but it does make sense to define
science this way.

## Organic VS Mechanic

I always found more inspiration for software in organic systems than buildings,
so it has a great appeal to me when Alan Kay talks about how great life is
at scaling. Not that metaphors and analogies with architecture are bad, actually
you can see architecture and design in life too.

The thing is that scaling mechanisms, at least on how we build them, is very hard.
His example is always the dog house, you can build a dog house from almost anything,
and it works. But if you think to scale it to become a cathedral you can't just
multiply the dog house by a hundred, it will fall because of very know physics rules.

This is the same way a lot of software is built, get something that works on a small scale
and just force it to scale until it crumbles. When it crumbles two things can happen:

* Admit you was wrong
* Build a pyramid on top of the fallen build and say that this is what you wanted to build

Sadly almost all the times we convince ourselves that what we wanted was the pyramid =P.
We got a lot better at building great structures, but still it does not come close as
the level of scalability of life.

OK, but what can be applied from life itself to software ? Well take the cell, it is the
perfect example of something completely encapsulated by its cell membrane, everything
that comes in and goes out follows the protocol enforced by the membrane, nothing
comes inside without inspection, and what enables this awesome level of composition is
the fact that they have a great generic interface between them, proteins/DNA.

On Roy Fielding [dissertation on REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.html)
he explains that the most important aspect of REST is the imposed constraint
of having the same generic interface between components,
to scale at the level of the web it was mandatory to be able to compose new
and old stuff without causing disruption.

The constraints may make some things harder to implement,
but the better the generic interface, and the simpler, less limitations will be
generated by imposing the generic interface.

Life teaches us that to a system to scale it needs the right interfaces and the right constraints.
All life on earth needs the same basic building blocks, this imposes limitations on how life can
manifest itself, but it allows an awesome level of scalability and composition between everything
that is alive.

We need great building blocks or we are doomed to build things that will never scale,
being able to identify a bad building block and invest on building better ones is extremely
rare on the software industry, where companies are usually just worried with quarter profits.


## Reinventing the flat tire

On the [ARPA / Xerox PARC culture](https://www.youtube.com/watch?v=wdHtYW_wcAs)
presentation, besides a whole lot of awesome ideas, Kay talks about how
we are constantly reinventing the flat tire, not the wheel. Reinventing
the wheel would be great actually, the problem is that we keep stuck on
wrong interpretations of really good ideas and keep reinventing them.

One current example that happened just now (2020) is a new implementation
of NodeJS fixing all the terrible mistakes of NodeJS, the whole Javascript
enterprise is a flat tire and we are still pouring a lot of resources and time
in reinventing this kind of stuff instead of going for something really better.
Even if it is better than NodeJS, it will be at best a fancier flat tire.

One interesting thing about this metaphor is the whole reasoning that it works
and a lot of people uses it, so it must be good. You can get around with a flat
tire and the most scary part is that if what you ever had is flat tires you will
think that it is normal the way a car behaves with the flat tires, it has
always been like that, you have no idea of how different and faster it will
be with actual good tires, so you may convince yourself that flat tires
are awesome. It is the same as the canyon metaphor, after people are inside it
they can't see outside it, it is like the canyon is the whole world.


## Point of view is worth 80 IQ points

TODO: Talk about correlation with good abstractions


## Catch phrases

* It is big meaning, not big data
* The best way to predict the future is to invent it
* Point of view is worth 80 IQ points

## AI/LLM

* [What does Alan Kay think about programming and teaching programming with copilots and LLMs of today ?](https://www.quora.com/What-does-Alan-Kay-think-about-programming-and-teaching-programming-with-copilots-and-LLMs-of-today)
* [Now that machines can generate and "understand" language, what would Alan Kay argue that are a set of first principles from which to rethink the human-machine interface?](https://www.quora.com/Now-that-machines-can-generate-and-understand-language-what-would-Alan-Kay-argue-that-are-a-set-of-first-principles-from-which-to-rethink-the-human-machine-interface)
