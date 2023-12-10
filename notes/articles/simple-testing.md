# Simple Testing Can Prevent Most Critical Failures

Notes from [Simple Testing Can Prevent Most Critical Failures](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-yuan.pdf).

The article focuses on assessing critical failures that happened on open source databases:

* Cassandra
* HBase
* Hadoop Distributed File System (HDFS)
* Hadoop MapReduce
* Redis

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

Now things get interesting :-).

## Error Handling 

I have a personal beef with how very little attention software engineers give to error handling.
Actually there is a whole school of thought that teaches software engineers that error handling
is not part of your "business logic" (whatever the fuck that is), it makes your code ugly and you should
find ways to keep your code neat and beautiful and hide the ugly error handling from it.

That was always to me the recipe for building garbage software, and it is everywhere, and I think one technique
that pushes software in this direction is exceptions. Languages that have exceptions have 2 control flows, instead of a single one.
One is the normal/beautiful/business logic? flow, the one you should focus. The other one is the exception control flow,
who just disrupts everything and starts jumping around up in your stack. It is basically a constrained/maybe type safe goto, but
with a lot of the disadvantages of goto's. Exceptions make it remarkably easy to just forget about errors, you can finally focus
on the features !!!

And then when something goes wrong your user/client can get a nice stack trace that will be meaningless for them.
Specially on CLI tools it is remarkable how this is pervasive, even on "professional" tools like gcloud/azure I was
exposed multiple times to extremelly long/noisy/confusing stack traces instead of some proper error message that could
help me. One example is the classical something was supposed to be X but it is Y (in Python), or is None, or some dict
access failing. The stack trace is very precise...but for someone who is just a user of a tool that doesn't say anything,
I don't even know if the problem is caused by some wrong input on my side or by some other internal error on the tool
itself (a bug). And it is my opinion that exceptions and its idioms do push people in that direction. Of course
erros can be ignored in any languages, but many make it easier (API designs can also do that).

I tend to prefer languages where error handling is done explicitely and it will be everywhere in your code, it should,
error handling is just as fundamental as the feature/business code. Compare the usual exception handling hand waving
thing with [this blog post](https://commandcenter.blogspot.com/2017/12/error-handling-in-upspin.html) for example.

Enough ranting, how does this relate to the article ? 

```
Almost all catastrophic failures (92%) are
the result of incorrect handling of non-fatal errors explicitly signaled in software.
```

And this is very serious/distributed software were people usually pay more attention to errors :-).
Just imagine how it will be in "usual" software were ignoring errors is the default.
Interesting that most of the failures are related to not recovering from errors that are recoverable:

```
This error need not be catastrophic; however in the vast majority of cases, the handling
of the explicit error was faulty, resulting in an error manifesting
itself as a catastrophic failure.
```

This involve thinking and designing errors, and you can't do that well if you see errors as an
annoyance or some sort of second class citizen in your design.

A good part of the errors are not even that complex:

```
35% of the catastrophic failures are caused
by trivial mistakes in error handling logic — ones that
simply violate best programming practices; and that can
be detected without system specific knowledge.
```

A lot of the problems are related to over catching exceptions. Exceptions optimize the design
towards aggregating errors and handling them in a single place (because spreading error handling
is bad/ugly remember ?), that works well when aggregation is possible and you can handle errors
out of context. When it isn't you run into problems (literal bugs or leaky abstractions since a
higher layer will need more details than it should about the implementation).

```
While in principle one would need
system specific knowledge to determine when to bring
down the entire cluster, the aborts we observed were all
within exception over-catch, where a higher level exception
is used to catch multiple different lower-level exceptions
```

You can design exceptions well too to avoid these issues, but it seems to me that the overall culture
around what makes them "great" doesn't push in that direction.

And out of curiosity, a TODO bringing down huge production systems :-)

```
Figure 9 shows an even more obvious mistake, where
the developers only left a comment “TODO” in the handler
logic in addition to a logging statement. While this
error would only occur rarely, it took down a production
cluster of 4,000 nodes.
```

So beware of "rare errors", they may be rare but the impact may be huge.

## Automation

There is great emphasis on automating checks to avoid bugs, something similar to linters but even more
advanced. Good tooling is welcomed but I ended up focusing on the nature of bugs that humans write
and why they do it, even on more "serious" software. Maybe AI tooling will help...or maybe not since most
of them are trained with our open source code, which is the same code they are trained, which will also
contain mostly bad error handling and TODOs on exception handlers =P.

So it still seems to be worthwhile to just develop a more disciplined/thoughtfull mind, around both testing and
error handling. And mistakes will happen anyway, but maybe less :-).
