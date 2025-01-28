# The Emperors Old Clothes

```
I conclude that there are two ways of constructing a
software design: One way is to make it so simple that
there are obviously no deficiencies and the other way is
to make it so complicated that there are no obvious
deficiencies.
```

Notes from [The Emperor's Old Clothes](https://dl.acm.org/doi/pdf/10.1145/358549.358561).

This one is a really good read that caused an impression on me for two reasons.
The first is the candor of the author on talking about his own mistakes. He basically tells a story where
they kept trying to implement a system and it only got more and more delayed and it took them 2
years to finally give up and deliver nothing. It has all the classics of frog boiling, they slowed
cooked themselves not perceiving that delivering would not be feasible, it was too much features
and too little hardware. And yet he admits his mistakes and was surprised he was not fired but
actually got even more responsibilities, which makes sense since he owned his mistakes.

The second point he makes is how it is impossible (or close to it) to design a good programming
language by commitee. He describes the slow and painful process of watching the language bloat
as each iteration of the commitee only adds more cruft to it. In this case talking about the new
revised versions of ALGOL:

```
Three months came and went--not a word of the
new draft appeared. After six months, in October 1966,
the ALGOL working group met in Warsaw. It had before
it an even longer and thicker document, full of errors
corrected at the last minute, describing equally obscurely
yet another different, and to me, equally unattractive
language. The experts in the group could not see the
defects of the design and they firmly resolved to adopt
the draft, believing it would be completed in three
months. In vain, I told them it would not. In vain, I
urged them to remove some of the technical mistakes of
the language, the predominance of references, the default
type conversions. Far from wishing to simplify the language,
the working group actually asked the authors to
include even more complex features like overloading of
operators and concurrency
```

And then a snippet of wisdom on not adopting new features too fast:

```
A feature which is omitted can always be
added later, when its design and its implications are well
understood. A feature which is included before it is fully
understood can never be removed later. 
```

There is a caveat though, not every feature can be added later. Some systems are built
around ideas that may be at odds with the future feature, making it hard to implement
it unless with clumsy hacks. To design is to constrain and constraints always make properties
emerge in a system, these properties will make some features easy to implement and others
impossible. The most recent case of this the whole Go parametric types implementation, which
doesn't seem to fit in the original language constraints (like operators are not first class
citizens) and in the end the whole thing feels like a hack, it may still be useful but it will
still be a hack and less expressive than languages that had parametric types at the core of the
language design from the start.
