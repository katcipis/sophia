# ORM (Object Relational Mapping)

This is something I don't have a whole lot of experience with, but have seen it
go wrong in some contexts. Also from the first time I was introduced to the concept
I was always suspicious about the impedance between objects and databases,
specially given my intuition that objects are about messaging and hiding state and
databases are basically about data (and some data models about the relationship between data).
In time it seems I was not completely alone in sensing the mismatch, although sometimes for different reasons.

One interesting problem that ORMs introduces is modeling
your data as a second class citizen that follows the design of objects, basically
it raises the question if you should model your data first and then design
the system around that, or design the system (with objects in this case)
and then model the data around that. It even makes me wonder if both should not be
completely decoupled. My experience is that yes. There may be some reasons to design your
logic/software in a particular way that is unrelated to the challenges on modeling data
in a way that is easy to access. Specially because "ease of access" is domain specific,
it means that it is easy to find (and potentially correlate) data in a way that is
useful to clients/analysts/etc. It could be argued that it is possible to decouple both even
with ORMs, but in my experience it rarely is, when ORMs are used the object model of the service
becomes the data model, most of the time in detriment to the search/access experience.

Interesting enough, most of the people that I know that are more data intensive,
know a lot about SQL and think a lot on terms of database schemas/data modeling, don't like 
ORMs. Probably because of that, they see the data design as first class and see access issues
as the main problem they need to solve, they start from there and when you are starting thinking
only about data and how to search/correlate data I don't think objects help, we do need a different model
to deal with this. One example of such model is the relational model. One can think they are related because
objects can "relate" to each other too, but I believe they really don't. Even when objects have a relationship
with other objects, in a well designed system that is usually hidden from anyone interacting with the object.
One example of such principle is the [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter).

My point here is that your mindset when you are designing objects is quite different (and to solve different problems)
than when you are designing data to be accessed. I'm talking a lot about access because the whole point (and the hard problem to solve)
of data is to be accessed efficiently, in useful ways...and in a way that scales and handles change/evolution well.
The problem is not storage, is the interface to access the stored data and how that evolves along time. So one can say it is all about access/search.

With objects you are thinking on how to isolate state, and even isolate/hide relationship of different objects through more high level/useful
interfaces. With data you are thinking about every single piece of data and all the relationships between them, how to evolve that, there is
nothing to be hidden here, you are working to make things accessible and know. For example there is no such thing as private data in
a database because that is mostly useless (in the sense that it can't be accessed by anyone, this is different from access control to specific
users etc, which of course it is useful), you are making data/state explicit and for the purpose of being accessed/manipulated.

Another problem with ORMs is the assumption that the ultimate way to design software is with objects.
What about functional programming then ? Or even other paradigms to design software. Just as it happens with design
patterns, ORMs give way too much credit to objects.

Some good material on the topic, usually more on the "why not" realm because
there is already too much material on "lets do it", usually using some logic like
"SQL much hard", "Deliver ASAP", etc:

* [The Vietnam of Computer Science](http://blogs.tedneward.com/post/the-vietnam-of-computer-science/)
* [Object-Relational Mapping is the Vietnam of Computer Science](https://blog.codinghorror.com/object-relational-mapping-is-the-vietnam-of-computer-science/)
* [Exiting the Vietnam of Programming: Our Journey in Dropping the ORM (in Golang)](https://alanilling.com/exiting-the-vietnam-of-programming-our-journey-in-dropping-the-orm-in-golang-3ce7dff24a0f)
