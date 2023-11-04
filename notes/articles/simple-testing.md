# Simple Testing Can Prevent Most Critical Failures

Notes from [Simple Testing Can Prevent Most Critical Failures](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf).

The article focuses on assessing critical failures that happened on open source databases.
The findings are revealing, the title already contains spoilers but the numbers are quite interesting,
indeed most failures could be prevented with better simple/isolated testing.
This has caught my attention because I had my share of experiences where people talk about some
part of a system is hard or impossible to test, for me this was most of the time bullshit and the
article brings this home. There is a very close to 0 chance that the software you are writing right
now is more complex than a distributed (or even a single node) database. So if even databases can
have distributed issues reproduced locally/isolated, so can your software.

Sometimes it is hard, I struggle with testing all the time, but we must see it for what it is, a failure
in imagination/creativity in design/testing. Never just give up and label a problem as "not testable",
because it most likely isn't. This reminds me a lot of the
[Test Driven Development for Embedded C: Building High Quality Embedded Software](https://pragprog.com/titles/jgade/test-driven-development-for-embedded-c/) book.
It seems impossible that you will be able to test well low level/hardware dependent code, and keep it efficient, but it is quite possible
if you think carefully about your design and know your tools well (like link time substitution).

It is true that each domain have different kinds of challenges on how to approach testing (or how much to test), but
there always a way and we should always look for it, unless you don't care about your software having critical failures (which is OK sometimes).

It did surprise me that most failure are that simple/deterministic even on such complex/distributed systems:

```
We show that the failures can be reproduced offline relatively easily, even though they
typically occurred on long-running, large production clusters. Specifically, we show that most failures require no
more 3 nodes and no more than 3 input events to reproduce, and most failures are deterministic. In fact, most
of them can be reproduced with unit tests.
```

There is also some praise for logging, good logging can take you a long way in a complex/distributed system:

```
We also show that the logs produced by these systems are rich with information,
making the diagnosis of the failures mostly straightforward
```

Nothing new, just reinforcing the basics :-).

On the the determinism of errors:

```
74% of the failures are deterministic — they
are guaranteed to manifest given the right input event
sequences. 

This means that for a majority of the failures, we only
need to explore the combination and permutation of input events,
but no additional timing relationship.
```

On reproducing errors:

```
A majority of the production failures (77%) can be reproduced by a unit test.
```

And on requiring "production data":

```
Specific data values are not typically required to reproduce the failures; in fact, none of the studied failures
required specific values of user’s data contents. Instead, only the required input sequences
(e.g., file write, disconnect a node, etc.) are needed.
```

This matches well my empirical evidence. The problem is usually the sequence of events/requests or the structure/nature of the
data, not the data itself. This is specially relevant to me because I'm seeing more and more developers that need a copy
of the production database in order to be able to develop and that is just lazy. You don't need production or production like
data most of the time, you just need to come up with permutations of different interactions and also boundary testing/corner
cases on your data structures. Being lazy is sometimes good, but in this case it makes you a worse tester/designer and also
makes things more expensive (and less safe, copying production data safely is tricky).
