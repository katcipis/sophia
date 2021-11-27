# The Dream Machine

Notes from [The Dream Machine](https://www.amazon.com/Dream-Machine-M-Mitchell-Waldrop/dp/1732265119).

# Math Formalism and Notation

Since I'm plagued by feelings of inadequacy (and as any good human,
quite self centered) anything that makes me feel less hopeless gets
my attention, and in this case it was the part of the book
that introduces [Norbert Wiener](https://en.wikipedia.org/wiki/Norbert_Wiener).

The guy has a very strong background in math and is extremely creative,
being able to envision things on the computer that most of his fellows
mathematicians failed to do. 

But what got my attention was his approach to math, even though he had an
explicit/strong background in formal math, he didn't liked to approach
math formally and specially disliked mathematical notation/syntax,
for him math notation was a necessary evil, and most of his mathematics
was done more guided by intuition and by thinking on physical terms,
instead of classical theory/symbols/notation.

I'm not even close to having a strong math background, but this does
give me hope on being able to use math usefully someday even though
I was never able to through a lot of the math formalisms. I specially
dislike math notation since it is usually very terse/dense, like a very
complex regex =P. And yet some things connected to math, like proofs by
induction and logical math, applied on computing, I was able to grasp
much more easily.

Math is very central to creativity and abstract reasoning, and now I feel
less hopeless because it does seem to exist different ways to leverage
that, not only the most classical way that I was never able to grasp
(and was always ashamed of not being able to).

One curious thing the book mentions, Wiener proclaimed that
[Courant](https://en.wikipedia.org/wiki/Richard_Courant) stole a lot
of his ideas. Later in his life Wiener wrote a fiction book that
depicted a math professor that made his name by stealing ideas
from his younger genius students, which does seem to correlate
with his personal experience with Courant. This is of course, his
side of the whole story =P.

# Top Down vs Bottom Up

When the book reaches the time period of 1960 it starts talking about
McCarthy/Lisp and some other interesting stuff that was happening around
this time. One interesting thing was an analysis on different ways to
design systems, the classic top-down vs bottom-up.

There is usually no right approach, since this is also related to how
people usually think about things. But the book gives a hint on why
each approach may be better suited for different kinds of problems.

When you have a problem more well understood, where you have a clear
vision of the whole structure, you can be top/down in a way that resembles
more a classic/hierarchical manager, you can delegate different parts of
the overall task to sub-modules easily because you have a very good view
of the whole problem and the structure of the solution.

For problems that are less understood, or that you have less understanding
about the solution structure, bottom-up will be beneficial, since with
bottom-up you focus on defining useful/small core blocks that you are
not sure yet how you are going to compose to solve the final problem.
The small blocks + composition allows you to explore more freely and
iterate faster.

On top of this overall idea, which I found intriguing/interesting, it was
also defended that Lisp was well suited for bottom-up design, it is fairly
easily to start with small functions and compose them into larger ones
incrementally, not all languages (if any) around 1960 would allow you
to approach problems that way.
