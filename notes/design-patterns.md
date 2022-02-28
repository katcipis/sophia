# Design Patterns

* [Rob Pike on patterns](https://youtu.be/5kj5ApnhPAE?t=301)
* [Alan Kay on patterns](https://www.drdobbs.com/architecture-and-design/interview-with-alan-kay/240003442?pgno=4)
* [Roy Fielding on patterns](https://www.ics.uci.edu/~fielding/pubs/dissertation/software_arch.htm#sec_1_6)

## Alan Kay on Patterns

Quote from [Alan Kay interview on Dr Dobbs](https://link.springer.com/content/pdf/bbm%3A978-3-319-90008-7%2F1.pdf):

```
The most disastrous thing about programming—to pick one of
the 10 most disastrous things about programming—there’s a very
popular movement based on pattern languages. When Christopher
Alexander first did that in architecture, he was looking at 2,000 years
of ways that humans have made themselves comfortable. So there
was actually something to it, because he was dealing with a genome
that hasn’t changed that much. I think he got a few hundred valuable
patterns out of it. But the bug in trying to do that in computing is
the assumption that we know anything at all about programming.
So extracting patterns from today’s programming practices ennobles
them in a way they don’t deserve. It actually gives them more cachet.
```

## Form Follows Function: Design should make domain clear

This is not a problem intrinsic on using design patterns, but 100% of the cases
that I was exposed to people that loved design patterns the design patterns
were the first class citizen. The first thing you would see when looking at
names of folders/modules/classes/functions/etc are the patterns names.

Factory this, Proxy that, ObservableSomething. That always felt confusing to me
because a pattern is something underlying a structure, it takes some time and
squinting to see the pattern is there, and the design patterns movement seem to
love them so much that they make it explicit, so instead of having to squint
to see the pattern I need to squint to understand what the code does. Like Kay
said, people ennoble patterns way too much.

It is a case of function following form, form or the pattern comes first, and
then follows the function. Which is a great way to make APIs/services that are
terrible to use/maintain, because the main purpose of code is to function, it
is not a work of art, it runs and does things for you, it can be beautiful but
the beauty should serve the functionality in a way that the beauty is in the
details, but at first it is just very easy to use and to understand what the
software is about (and it should not be about patterns or MVC or whatever).

There is some interesting information on making things domain based on
[CUPID](https://dannorth.net/2022/02/10/cupid-for-joyful-coding/#domain-based).
