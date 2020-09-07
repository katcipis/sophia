# Why Programming is Hard ?

This is something that I stumble often, specially given how much movements
there are to bring programming to everyone. It is kinda tough to reason
about this because people will usually label you as elitist if
you try to argue that programming is not for everyone, which seems
rather odd since physics and mathematics don't seem to be for everyone
and everyone is OK with it. Probably because there is a lot of money
on programming and a lot of talk on how it is the future, so making
people feel excluded from the future would be bad. But the idea that
it is the future makes very little sense to me. Mathematics changed
human life, and yet it didn't became THE future, it is still just
a tool and a lot of people who are really bad at it (like me) can
still live just fine.

But anyway, I started to make some notes on why it seems to me that
programming is very hard, I don't do this because I want to avoid people
getting in, I do this because it is hard to me and maybe I don't want to
feel like I'm stupid. Since I'm clearly biased I try to use references
as much as possible.


# Systems Thinking and Abstract Reasoning

From [how](https://internetat50.com/references/Kay_How.pdf):

```
Many of what we consider our “immense challenges” have
come about from our genetic heritage of
hundreds of thousands of years of coping at smallscales,
projecting our beliefs on our perceptions, etc.

Even what we call “modern thinking” was a series ofinventions.
“Normal thinking” for us is “rememberingand recalling” for short
term concerns, often in the formof proverbs, stories,
and simple rules and rituals.

As newways to think were gradually invented they had to coexist
in human minds more like new tools on old mindsrather than to
create brand-new greatly improved minds.  
``` 
Thinking about systems in abstract ways is very challenging for
humans in general. This doesn't change very fast, since it is tied to
thousands of years of evolution, and we are not even close to properly
teaching it in school for kids (which is the life work of Alan Kay, changing
education for kids).

So we are pretty far from making this a basic skill that most humans have
and it is at the heart of programming (which is essentially organizing and
formalizing systems at a very abstract level).

Also it may not even be possible, mathematics is around for quite some time,
it is taught in schools, and yet most people don't get it and are not very good
at it. I suppose that if this kind of change is possible it will take centuries.

Technology creates the illusion of speed, human brains are not THAT amazing
neither that fast, we are quite limited and embracing that can help a lot.

More on that topic:

* [The humble programmer](http://www.cs.utexas.edu/users/EWD/transcriptions/EWD03xx/EWD340.html)
* [Programming Considered as a Human Activity](https://www.cs.utexas.edu/~EWD/transcriptions/EWD01xx/EWD117.html)


# Scaling

The same problem, at a different scale, differs greatly and to exemplify this
architecture is of great help.

From [how](https://internetat50.com/references/Kay_How.pdf):

```
Anyone can build a doghouse from almost anything.Almost no one can build one just 100 times larger 
```

This is related to the fact that not all physical forces
will scale linearly with size:

```  This is because the mass of the doghouse has increased
by one million, and the strength of its material
(more or less proportional to cross section) have only
increased by a factor of 10,000 — so the structure has
gotten weaker with respect to gravity by a factor of 100.

The New Orleans Superdome is about 200 times the sizeof the doghouse,
and had to be made very differently!
Infact, scaling things up only 10 times larger is very often quite difficult  
```

In the case of software we have even a worse situation,
because we have even less experience reasoning about purely abstract systems
and will have even less idea on how to deal with scaling,
like what are all the forces involved on software systems and how do they
scale with system size ? (like impact on human cognition, etc).

# Attention to Detail
