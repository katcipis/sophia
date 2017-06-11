# A Relational Model of Data for Large Shared Data Banks

These are the notes from the great article
[A Relational Model of Data for Large Shared Data Banks](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf).

Sadly I'm almost sure that I did not understood most part
of the article, but I will try to register here some key points that
seems interesting to me.

The most interesting part of relational theory is why it has been
conceived:

```
Activities of users
at terminals and most application programs should remain
unaffected when the internal representation of data is changed
and even when some aspects of the external representation
are changed. Changes in data representation will often be
needed as a result of changes in query, update, and report
traffic and natural growth in the types of stored information.
```

Why this is interesting ? Well, one of the best selling points
of NoSQL databases is that they are easy to evolve without breaking
applications. Well, the main objective of relational theory was to
enable exactly that. Not just internal representation, but also
changes on external representation.

There will be some changes on external representation that will
make applications break, but this is just what happens with
NoSQL too, some changes will inevitably break, but the idea to
avoid that and enable smooth evolution of the database as it's
usage grows was a first class concern on the relational theory.

This is also made very clear here:

```
Accordingly, it provides a basis for a high level
data language which will yield maximal independence between
programs on the one hand and machine representation
and organization of data on the other. 
```

The idea seems to enable programs with different purposes
to use the same shared data bank. These days this seems
like a bad idea, and incrementally we are repeating the same
mistakes that people before us had made, because avoiding
this will result on data duplication, and managing that
on a lot of contexts is pretty hard. But the idea to organize
data as documents with hierarchical structure, and duplicate
data, are not novel and they just have serious limitations
(and costs).

Not that they are always bad ideas, but they are not the panacea
that is sold to achieve scalability and awesome systems
(it is not even new).

It is not just the consistency problem, costs also rises with
duplication, and storage is becoming cheap, but it is not free.
And the costs of maintaining multiple databases are far from
just storage space, there is also operational costs (much higher than
the storage, depending on the scale).

The most predominant model these days (2016/2017), at least among devs,
is the document model, which is basically an hierarchical model.
The section 1.2.3 Access Path Dependence explores how this model
can easily generate problems as the demands that you have from
your database evolve.

On a hierarchical model you will always have a dependence on the
access path. For example, lets say that a person has an address,
like this:

```
{
    name: "bla",
    address: {
        "street" : "lala",
        "number" : 666
    }
}
```

The first problem with committing to an hierarchy is coupling.
Lets say you just want to load all addresses available on the system,
to do some analysis, you will have to know (and couple) with the 
person set, and the access path to get to addresses on each person.

To avoid that on a document model you could generate duplication, like
maintaining a copy of all addresses on a set that has only addresses, or
to avoid duplication altogether you would need to store a id for the address
inside the person, that is just a reference to the address stored on
the addresses set, which is basically what the relational model proposes
(but implemented manually by you).

As your database evolves, now other concepts also have addresses related
to them, like companies, and it is not impossible for a company to have
the same address of a person (specially depending on the level of detail
of the address), again you have the duplication problem, or build some
form of manual relation.

Sometimes duplication may not be a problem, but it is interesting that
the relational model was proposed to allow a system to evolve without
requiring duplication of data and without requiring application to
be fixed. Some schema changes can happen without breaking any application.

The paper does a great job explaining the relational model as a predicate
calculus applied to sets. If the sets are simple and normalized I fail to
see any kind of limitation on what can be done if you have a good way
to describe operations on top of these sets to create new ones, the
possibilities seems to be pretty huge, and it is pretty flexible.

The paper even introduces the idea of "expressible set", which is the set
of all sets that could be expressed on the system, which is pretty big.
It is the idea of composing on N different ways sets that are very
simple (where simple implies the normalization process). It makes a lot
of sense, even with my very little experience with huge datasets, because
on software this kind of functional composition is already a good idea
to achieve maximum flexibility.

As said on the start, there is also a lot of other important concepts
that I was not able to fully digest to the point where I can provide
an opinion, more time and experience will be required.

On the consistency subject, it is interesting that the author
introduces the idea of inconsistencies that are unsolvable without
manual interference from the user:

```
The point is that the system will
normally have no way of resolving this question without
interrogating its environment (perhaps the user who created
the inconsistency ).
```

This is an approach used by a lot of databases to solve inconsistencies,
specially the highly distributed ones that have more space for inconsistencies
creeping in.
