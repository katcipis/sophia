# ORM (Object Relational Mapping)

This is something I don't have a whole lot of experience with, but have seen it
go wrong in some contexts + I was always suspicious about the impedance mismatch
between objects and databases, specially given my intuition that objects
are about messaging and hiding state, and databases are basically..about making
state explicit/public in some sense. In time it seems I was not completely
alone in sensing the mismatch, although for different reasons.

Besides the mismatch on thinking on hiding state VS modeling explicit data schemas
based on hidden state, one interesting problem that ORM introduces is modeling
your data as a second class citizen that follows the design of objects, basically
it raises the question if you should model your data first and then design
the system around that, or design the system (with objects in this case)
and then model the data around that.

Interesting enough, most of the people that I know that are more data intensive,
know a lot about SQL and think a lot on terms of database schemas, don't like 
most ORMs, probably because of that, they see the data design as first class,
they start from there. Some code generation could help them, maybe, but they
want to start the design from the data (which seems sensible in some scenarios).

I don't think there is a definite answer, like always do X,
but it is important to understand what is best for your problem, because it
affects how ORM can help you or destroy your life. This is related to the question
of "should I always use objects ?", if there are no objects, there is no
mapping to be done, there should be some interesting solutions to this kind
of problem on the functional programming landscape (unaware at the time I'm
writing this).

Some good material on the topic, usually more on the "why not" realm because
there is already too much material on "lets do it", usually using some logic like
"SQL much hard", "Deliver ASAP", etc.

* [The Vietnam of Computer Science](http://blogs.tedneward.com/post/the-vietnam-of-computer-science/)
* [Object-Relational Mapping is the Vietnam of Computer Science](https://blog.codinghorror.com/object-relational-mapping-is-the-vietnam-of-computer-science/)
* [Exiting the Vietnam of Programming: Our Journey in Dropping the ORM (in Golang)](https://alanilling.com/exiting-the-vietnam-of-programming-our-journey-in-dropping-the-orm-in-golang-3ce7dff24a0f)
