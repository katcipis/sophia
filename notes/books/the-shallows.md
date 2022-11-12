# The Shallows

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
