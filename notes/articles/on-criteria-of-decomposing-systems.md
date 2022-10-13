# On the Criteria To Be Used in Decomposing Systems into Modules

The [On the Criteria To Be Used in Decomposing Systems into Modules]([https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf))
article is just pure gold on good criteria to decomposing systems into modules (using assembly
on the examples, kick ass !!)


## Isolate system from hard design choices

The main reason that I got from the article that was pretty new to me was this one.
Basically hard design choices (like how to persist data) have a good chance to change
on the future. When they do change, what is the impact on your system.

He proposes two designs and shows the impact of changing some design decisions on both.
On the flow oriented design (the flow of the application), almost all changes implied
changing the whole system.

On the second design, done to isolate the implementation details from the rest of the system,
you usually just had to change one module per change made (reminds me of orthogonality).

On the end, change is the driving factor to decide if a design is good, how well it can
handle changes defines the quality.


## What about single responsibility ?

There is the old Single Responsibility principle, from SOLID. It has its merit since
it is too trying to optimize your code to change, but it is a lot harder to understand
than just isolating the system from hard design choices.

Basically it advocates that each module must have only one reason to change, but this
is inherently ambiguous, what is a reason ? What is a responsibility ?

The best explanation I found for this is based on the nature of the reason, or the
owner of the reason. Sometimes this is pretty hard to identify, so Im going to use a
very simple example, database and UI.

Database schemas will change because of the desires of a DBA, wanting to organize its
database better.

UI will change because of a product owner not satisfied with the UX, or some designer
that wants to implement something fancier.

If you don't isolate this two things from each other you will have serious problems.
The first one is the surprise effect, no one expects to a change on the UI to break
the database (like saving garbage there). It makes you look like an idiot.

This will be exacerbated by another problem, the pace of change (which is mentioned on
Building Microservices as one of the reasons to split a service in two).
The UI usually has a much greater rate of change than the database, so every time something
changes on the UI you have a great deal of possibility of breaking something related to
the database (which probably will not change too much, or not change at all).

Difference on rate of change is usually is a hint that two concepts are not related, and actually
should be split from each other completely.
