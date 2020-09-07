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
Anyone can build a doghouse from almost anything.
Almost no one can build one just 100 times larger 
```

This is related to the fact that not all physical forces
will scale linearly with size:

```
This is because the mass of the doghouse has increased
by one million, and the strength of its material
(more or less proportional to cross section) have only
increased by a factor of 10,000 — so the structure has
gotten weaker with respect to gravity by a factor of 100.

The New Orleans Superdome is about 200 times the sizeof the doghouse,
and had to be made very differently!
Infact, scaling things up only 10 times larger is very often quite difficult  
```

In the case of software we are in an even worse situation,
because we have even less experience reasoning about purely abstract systems
and will have even less idea on how to deal with scaling,
like what are all the forces involved on software systems and how do they
scale with system size ? (like impact on human cognition, debugging,
performance, etc). My personal experience with this is that people
in general, and even software engineers, often doesn't have a good mental
model of how scaling affects software, they seem to believe that just doing
what they did at a smaller scale X 100 will work out.

Thinking about scale is something that makes it easier to understand at
which scale it may be possible for anyone to program too. To think that
all that you have to do is to learn a programming language is to be
able to build dog houses, that is perfectly fine and useful, but that
doesn't make you an engineer/architect able to build bridges, because scale
matters.

This is also a great argument to avoid working at scale if you don't need too.
Don't build skyscrappers if you just need a dog house.


# Attention to Detail

Attention to detail is one I got from the Coders at Work book, exactly when
someone was questioned about making programming something that is for everyone.

From the [Coders at Work](http://www.codersatwork.com/) book:


```
Seibel: Yet lots of people have tried to come up with languages or
programming systems that will allow “nonprogrammers” to program. I take
it you think that might be a doomed enterprise—the problem about
programming is not that we haven’t found the right syntax for it but that
people have to learn this unnatural act.

Steele: Yeah. And I think that the other problem is that people like to
focus on the main thing they have in mind and not worry about the edge
cases or the screw cases or things that are unlikely to happen. And yet it is
precisely in those cases where people are most likely to disagree what the
right thing to do is.

Sometimes I’ll quiz a student, “What should happen in this case?” “Well,
obviously it should do this.” And immediately someone else will jump in and
say, “No, no, it should do that.” And those are exactly the things that you
need to nail down in a programming specification of some process.

I think it’s not an accident that we often use the imagery of magic to
describe programming. We speak of computing wizards and we think of
things happening by magic or automagically. And I think that’s because being
able to get a machine to do what you want is the closest thing we’ve got in
technology to adolescent wish-fulfillment.

And if you look at the fairy tales, people want to be able to just think in
their minds what they want, wave their hands, and it happens. And of
course the fairy tales are full of cautionary tales where you forgot to cover
the edge case and then something bad happens.
```

This makes sense to me because even among "professional engineers" one 
of the top subjects that is usually completely ignored is error handling.
On top of that we have whole "new" ideas centered around ignoring resiliency
issues and faul tolerance and let it be automatic, like service meshes.
So it seems like a true clear bias that people usually don't do that
very well (and the best programmers do).
