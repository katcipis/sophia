# Good Architecture

Bases:

* https://vimeo.com/43612849


## Principles

* Makes intention of the system clear (not tools)
* Allows to defer tools decisions
* Focus on the core logic of the problem you are solving (not related to tools)
* Decouples logic from UI
* Decouples logic from delivery mechanisms (HTTP/RPC/WHATEVER)
* Decouples logic from persistence (the database, of course)
* Decouples logic from frameworks of any kind

About intention, since the metaphor used is about architecture, the plant of a building
usually allow you to infer what kind of a building it is (school, library, church).

Architectural diagrams and project directory layout should do the same. Im not interested
on the framework chosen, or if the system is a web system using Rails. First I want to
know the intention of the system as a whole, what it does, them I can search for how.


## Procedures inside the database ?

* Putting procedures inside the database is the utmost level of coupling with the database
* It is a tradeoff from latency to strong coupling, you are married to the technology now
* If you need to change the database for some reason, you are going to have a bad time (eg. Neoway)
