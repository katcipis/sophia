# Algorithms to Live By: The Computer Science of Human Decisions

As the title makes clear this book is about making comparisons
to common computer science algorithms and human behavior.

There is a lot that can be taken from computer science and math
to the day to day life, for example one of the first topics
of the book is the [optimal stopping](https://en.wikipedia.org/wiki/Optimal_stopping)
problem, which is a classic from math that a lot of people
don't know but can be really useful if you are looking for an
apartment and want to know how much time you should spend just
looking before committing to an apartment.

It goes a great deal on sorting and searching also, remembering
that sorting only makes sense when you actually need to search
and the searching cost is not negligible. One simple example
is your personal library of books, it will usually be not
that big and sorting it will bring you little value given the
effort (at least O(n logn)). But for a huge library it is
essential. The library subject is very rich because it encompasses
both sorting, searching and caching. Caching was one of the
most interesting topics to me and merits its own section,
not because of the computer science aspect of it but because
it made me feel less lame as I get old =P.

# Caches

The caches part is great because the connection with
the real world and physics is impressive. The first idea
is to consolidate the notion that caches will always be
necessary, this goes back to the 60's and theories
of memory hierarchies. Why would you always need to
build a hierarchy of memory ? Because of limits on
how much information you can store into a physical
phenomenon given the size it will take to store it.
And as you get more distant from where the data is
it will be intrinsically harder to access it as fast
as the data stored right next to you. This is very
interesting stuff because it forces you to open
abstractions and really think how information is stored
physically, since the limits comes from there.

It does not matter how good we get at storing information,
increasing density (storing more data per millimeter for
example), you will always hit a limit where to store more
data you will need to send the data further away geographically.

Moving data further away has the immediate physical implication
that moving data around will get harder and will have added latency.
Even if you get awesome throughput (at a cost), at least latency
is inevitable given limits like the speed of light. Specially
with trends like big data (everyone wants to store everything),
it gets more obvious that caches are here to stay, because
when you want to be responsive when analyzing data this data
must be near the person/machine who requested it, since it is
physically impossible to leave ALL the data near EVERYONE you
will end up with some caching mechanism that will help
with that. Another example used on the book is
CDNs (Content Distribution Networks), which
alleviate the hardship of delivering content to the
entire world with reasonable performance (in the book
the author talks a lot about Akamai =P).

A great analogy to how this works in the analogic world
is when you have a desk in a library. The books that you
leave on your desk are your cache, it is extremely fast to
search through them but it is physically impossible to
put the entire library in your desk, hence you have a cache,
a small portion of data has fast access and when you have a 
cache miss you have to get up and go search on the library
(the slow memory).

The really interesting insight to me is that caches are not just
an economical way to store most used data that is too expensive
to be used to store all data. It is physically
impossible to have memory fast as caches in most cases
even if you had money to pay for the fast hardware.

Take the library example, if you build an enormous table
(since caches are fast lets make everything a cache) then
it would be slow to search through it anyway, it lost its purpose
of being fast because to be fast it NEEDS to be small. So you
can have a cache and be fast on a lot of access or not have it
and always be slow, but there is no option that will deliver
you cache like performance to all accesses unless your dataset
is small.

The same thing happens to processors. The first naive impression
that you get when you see how fast caches are on processors is
"Why all the memory in the computer is not made of caches ?". The
naive answer is that it would be too expensive, because the SRAM
used on caches is really expensive, but that is not the case. It is
physically impossible to have memory operating so close to the clock
of the processor but being physically distant from the processor
itself, the signal clock literally takes time to travel in space,
so even if you wanted to pay for a lot of SRAM the distance
between the SRAM cells and the processor would make it impossible
to operate in synchrony with the processor. So physically, just
as in the case of the table on the library, caches on processors
need to be small, not just because SRAMs are expensive, because
physically being small is what enables them to be faster.

There is also a good insight on realizing that even tough
a lot of abstractions provided by the cloud can be useful
you should take care because when performance is an issue
you usually need to understand things in details, sometimes
physical details. Also it makes you think if it is a good
idea to keep huge datasets, the smaller the dataset the
easier will it be to have good performance analyzing it.
Do you really need all the data you store ?

This pattern of having smaller fast memory to work on immediate
things and a lot of slower memory also happens in nature, in
the most obvious organ that we think about when thinking
about memory, the brain.

## Caching and the Brain
