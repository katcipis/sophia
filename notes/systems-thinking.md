# Systems Thinking/Design

Some interesting material on systems thinking/design. Not focused
necessarily in software, but a more broad view of the matter.

## Systems design explains the world: volume 1

Can be seen [here](https://apenwarr.ca/log/?m=202012), from Apenwarr.
Very interesting post, digs a lot on the fallacies of full decentralization/
flat structures, for example:

```
"This apparent lack of structure too often disguised an informal,
unacknowledged and unaccountable leadership that was all the more pernicious
because its very existence was denied."

"Informal, unacknowledged, and unaccountable" control is just as common
in distributed computing systems as it is in human social systems.

The truth is, nearly every attempt to design a hierarchy-free, "flat" control
system just moves the central control around until you can't see it anymore.
Human structures all have leaders, whether implicit or explicit,
and the explicit ones tend to be more diverse
```

It also provides some great examples of computing/social systems
that are actually hybrid, instead of fully decentralized. It does
kinda defend that there is no such thing as full decentralization
in an actual working system (or it is very rare and can be
dangerous given hidden power structures).

```
The web depends on centrally controlled DNS and centrally approved TLS
certificate issuers; the global Internet depends on a small cabal who sorts
out routing problems.

Every blockchain depends on whoever decides if your preferred chain will fork
this week, and whoever runs the popular exchanges, and whoever decides whether
to arrest those people.

Distributed radio networks depend on centralized government spectrum licenses.

Democracy depends on someone enforcing your right to vote.
Capitalism depends on someone enforcing the rules of a "free" marketplace.
```

On truly distributed/de-centralized existing:

```
Truly distributed systems do exist. Earth's ecosystem is perhaps one
(although it's becoming increasingly fragile and dependent on
humans not to break it).

Truly distributed databases using Raft consensus or similar algorithms
certainly exist and work. Distributed version control (like git) really is
distributed, although we ironically end up re-centralizing our usage of it
through something like Github.
```

And some good final advice on how to deal with the whole problem
when designing systems (also social ones):

```
In systems design, there is rarely a single right answer that applies
everywhere. But with centralized vs distributed systems, my rule of thumb is
to do exactly what Jo Freeman suggested:

at least make sure the control structure is explicit.
When it's explicit, you can debug it.
```
