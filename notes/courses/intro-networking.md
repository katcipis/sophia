# Introduction to Computer Networking 

Notes from the course
[Introduction to Computer Networking](https://lagunita.stanford.edu/courses/Engineering/Networking-SP/SelfPaced/about)

# End to End principle

The end to end principle is a systems communication principle
that dictates that every feature that can be safely implemented
on the endpoints should be done that way, instead of building
a smart network, build something very simple that enables
endpoints to build more complex stuff.

This reminds me a lot of the smart endpoints / dump pipes
principles for services.

Sometimes it is hard to see the benefits from it. The largest
network on the time was the telecommunication network and it was
based on a smart network with extremely dump endpoints (the old phones).

On the design of the IP protocol this idea of a dump pipe was applied,
the IP protocol guarantees almost nothing. It does not guarantee delivery or
order. At the time these seemed like pretty basic stuff, but if features
like retransmission and acks where embedded on the network it would be
very hard or even impossible to build systems like VoIP, where low latency
is more important that reliability.

It is hard to predict how your communication protocol is going to be
used, to avoid problem it is best to keep it simple since it will
be the base for the communication of a lot of different applications.

Seems like a good principle to be applied to any distributed system,
when using message brokers it is good to have this in mind.

A real life example of this is the wireless protocol that does retransmission
at the layer level since there is so much loss on wireless. This enables
recovery from losses with low latency (at layer 2), but for protocols where
retransmission of stale data is harmful this is a bad choice and there is
no way around it on wireless since it is enforced at the link layer.
