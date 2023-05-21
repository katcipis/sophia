# The Shallows

Notes from the book [The Shallows: What the Internet Is Doing to Our Brains](https://www.amazon.com/Shallows-What-Internet-Doing-Brains/dp/0393339750).

# Neuroplasticity

One of the most interesting points made in the book is neuroplasticity.
Actually most of the book is the author establishing the premisse that
tools shape how we think and that this phenomenon also happens with adults.

This reminds me a lot about [The Humble Programmer](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html)
article, it goes like this:

```
Now for the fifth argument. It has to do with the influence of the tool we are
trying to use upon our own thinking habits. I observe a cultural tradition,
which in all probability has its roots in the Renaissance, to ignore this
influence, to regard the human mind as the supreme and autonomous master of
its artefacts.

But if I start to analyse the thinking habits of myself and of my fellow human
beings, I come, whether I like it or not, to a completely different conclusion, viz.
that the tools we are trying to use and the language or notation we are using
to express or record our thoughts, are the major factors determining what we
can think or express at all!
```

This made me reflect on how the languages we use when programming shape our
thought process, the book extended this to thinking about all sorts of
other tools we use on our daily lives that shape our thoughts.

The author goes on mentioning a lot of inventions like the typewriter, printing
press, alphabet/writing, etc. All shaped our behavior and our societies.

One example is Nietzsche, by the end of his life he was almost blind and was only
able to write using a typewriter and he mentioned in a letter to a friend how
the typewriter changed gradually the way he writed. It seems counter intuitive
since a typewriter is "just" a tool, and yet our brains are wired in a way that
tools become extensions of ourselves, so every tool we use shapes us back.

In that sense being human is truly amazing, we are capable of shaping things that
then shapes us right back. This is the essence of the meta circular evaluator from lisp
and the [Drawing Hands](https://en.wikipedia.org/wiki/Drawing_Hands) from Escher.

# The Internet

A lot of the book was not new to me, but he mentioned some research that I was unaware.
The whole point of the book is how the Internet, in exchange for everything it
gave us, took a good part of our capacity to concentrate and do reading for
longs periods of time. Everything is cut into 5/10 min bites and consumed that way.
A lot of the content is also purely visual, reducing how much we do read.
And all that has influence on how we learn and how we think.

One interesting research involved people studying a topic. Group A read in a more
classical way, like a book, while Group B read something that contained hyperlinks.
Intuition would say that hyperlinks are good, because you use them to deepen
your knowledge about the topic. And yet access to extra information in form of
links reduced the capacit of people to absorve and remember the material.
The shift of context in jumping around, even if it is for content related to
what we are reading, affect how much we are understanding/remembering something.
Like you optimize way too much for fast thinking and lose slow thinking.

The author also talks about his personal experience, on how hard it was to
write the book because he had a hard time concentrating. He ended up renting
a small shack in a remote area and wrote the book with very limited internet
access. This kind of experience is very subjective/qualitative, and yet I have
similar feelings that it gets harder and harder to do some quiet slow reading/thinking.

# Writing

And yet, the book presents some information that made me think and kinda goes against
the book itself, in the sense that sometimes it seems that some new technology will
make us less, but ends up making us more in different ways.

This is the history of the invention of the alphabet and writing. He focused on how it
started on Greece and mentioned how Socrates was concerned that writing was going
to dumb us down and make us more shallow, because after we are able to write down
things we don't need to remember things anymore. We would rely too much on the
written word and would end up becoming forgetful beings.

Maybe we did lose some of our capacity to remember things when we abandoned oral
tradition. And yet it is indisputable that every civilization that developed
writing and migrated to it from oral tradition evolved much faster and eventually
replaced the societies that relied only on oral tradition. So writing, even
with its trade-offs, is much more of a win than a lose.

And why is that ? the problem is interpreting the different ways that technologies/ideas
affects us. The intuition that writing will make you remember less, because you can just
dump ideas free your mind, does make sense in some way. But what about how writing
itself as a way to think and refine your own ideas ? And the fact that you can review
those brainstorms that you just did with yourself and evolve them, expand to involve
others etc. The fact that writing transcends time, transcends your own death.

This made me think a lot and realize that maybe the author is doing with the Internet
what Socrates did with writing. And he goes on talking about the printing press. When
the printing press started people started complaining that having the ability to
print so much content was lowering the quality of the content, because now people
were able to just publish all sort of stupid stuff.

This one made me think a lot, because I used to have similar feelings with the
Internet, but even though most of what you see may be garbage/stupid, the fact
that someone really smart can publish something very easily is always a win
(just as it was with the printing press).  We may need to build better filters,
there are challenges, but easier access to information seems like a win.

## Dangers of Expert Group Thinking

There is another instance of the dangers of expert group thinking. Group thinking is
dangerous but expert group thinking is the worse. And even though today it is well
accepted that adults also exhibit neuroplasticity this was not well accepted
by specialists for decades, and this form of group thinking kept science from
progress for quite some time (1920-1950). It is odd to me how even on science dogma always
finds a way to slip back into human behavior, we are awesome but at the same
time we seem to have really bad brains (evolution is messy).

How did they stop progress ? by not allowing anyone to think different. People
trying to do research on neuroplasticity would get no funding at all, because
the scientists "in power" would discredit them (with no scientific basis).

# Removing all effort is an anti-pattern

The final chapter of the book mentions a very interesting research paper called
[The Paradox of the Assisted User: Guidance can be Counterproductive](https://www.researchgate.net/publication/221519490_The_Paradox_of_the_Assisted_User_Guidance_can_be_Counterproductive).

The gist of the experiment is that people had to solve a set of increasingly
hard puzzles in a computer. The puzzle solving software has 2 implementation.

One is a very basic software that allows you to solve the puzzle and that is it,
it is mostly an interface and the human needs to do all the heavy lifting.

The other one is a more sophisticated version of the solver that provides a lot of
help, hints, etc. Basically tries to help the user as much as possible.

The interesting finding on this study is that people using the more sophisticated
software started solving puzzles faster, but as soon as the puzzles got more complex
they stagnated and started to get more and more slow, to the point where they
got considerably slower than the people using the very simple/unhelpful software.

What was observed is that people who used the helpful software concentrated less
and they were not thinking strategically, thinking ahead, they would just try
semi-random things until they where able to solve the harder puzzles,
taking longer in the end, or get stuck.

They also called the people to solve the puzzles again 8 months after the
experiment and in this second run the people using the simpler software were
twice as fast as the ones using the more sophisticated version.
In general people using the simpler software were more:

* Engaged
* Focused
* Strategical
* Direct and economical solutions
* Imprint of knowledge

The more people depended on guidance the less engaged they were at the task
and the less they ended up learning.

I think this has serious implications on software development too, a lot of what
we do is essentially solve puzzles, in a way, when we try to come up with designs
to accommodate features, re-design things, etc. If even for simple puzzles more
intelligent and sophisticated tools can shape how we think in a way that it dumbs
us down and makes us faster for simpler things but slower or unable to tackle harder
problems, imagine for more sophisticated/harder puzzles.

We are full throttle on the desire to remove all effort on software development, with
a plethora of sophisticated/graphical environments and recently AI assistance
like Github CoPilot, but how far these sophisticated helpers will really help us
deal with true complexity ? Does this kind of tooling scale to hard problems ?
Or do they just make us faster at building dumb shit (also making us more dumb
in the process) ?

It is really hard to not draw a comparison between the whole experiment and
really complex/graphical environments (IDEs) VS simple text editors, because
it captures the same essence as the experiment, some very simple software that
does very little to help you VS something very complex and rich that tries to
make things as easy for you as possible all the time.

Of course it is still hard to come up with an answer to this question, what is
the perfect amount of helpfulness, or else one could argue that the true
simple interface were punch cards, not text editors, so where to stop ?

Well I have no idea, but all this got me thinking a lot about this, and on the
merits of very simple environments like acme, that even though are quite simple
have a lot of flexibility and power too, but in a way that it is empowering instead
of doing your thinking for you. Even the discussion on acme of syntax highlighting
may be related to how our interfaces shapes our cognition. People using acme say
that after you get used to the lack of syntax highlighting you actually can focus
better, and this research may be evidence that this could be true. I used acme for
almost an year, I did get used to the lack of syntax highlighting, but there was
so much noise in my life at that time that I can't draw a conclusion from it,
would need to try again, maybe someday :-).

Another topic that comes to my mind on the debate of doing work in your brain
(internalizing) vs using rich tools (externalizing) is debuggers. It is not like
I never use debuggers, but some people say they are essential. And yet some smart
people doing "modern" computing still defend that they are not that essential, that
what is more essential is the internalizing process. Quoting directly Rob Pike
from [The Best Programming Advice I Ever Got" with Rob Pike](https://www.informit.com/articles/article.aspx?p=1941206):

```
A year or two after I'd joined the Labs, I was pair programming with Ken Thompson
on an on-the-fly compiler for a little interactive graphics language designed
by Gerard Holzmann.

I was the faster typist, so I was at the keyboard and Ken was standing behind
me as we programmed. We were working fast, and things broke, often visibly—it
was a graphics language, after all.

When something went wrong, I'd reflexively start to dig in to the problem,
examining stack traces, sticking in print statements, invoking a debugger,
and so on.

But Ken would just stand and think, ignoring me and the code we'd just written.
After a while I noticed a pattern: Ken would often understand the problem
before I would, and would suddenly announce, "I know what's wrong."

He was usually correct. I realized that Ken was building a mental model of the
code and when something broke it was an error in the model.
By thinking about *how* that problem could happen, he'd intuit where the model
was wrong or where our code must not be satisfying the model.

Ken taught me that thinking before debugging is extremely important.
If you dive into the bug, you tend to fix the local issue in the code,
but if you think about the bug first, how the bug came to be, you often find
and correct a higher-level problem in the code that will improve the design
and prevent further bugs.

I recognize this is largely a matter of style. Some people insist on
line-by-line tool-driven debugging for everything. But I now believe that
thinking—without looking at the code—is the best debugging tool of all,
because it leads to better software.
```

This may also be related to this research and how tooling shapes how we think,
or how we may end up not thinking which is the biggest danger.

Finally this also reminds me of the no-code solutions to building software.
In my opinion they map perfectly with the research in the sense that it is very
easy to start, you can build things much faster while the problems are simple
enough, but when you reach a plateau you are in trouble, and it is not just about
the limitations of the tool itself, it is also about the limitations the tool
created on you, which is the more dangerous part of the whole thing.
