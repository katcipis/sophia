# QUIC

QUIC is a transport protocol (layer 4) which overlaps in some features
with TCP, specially in the idea of providing as an abstraction
streams of ordered data being transmitted, but it has a very different approach
to connections and streams and some interesting implementation details
that makes it an interesting alternative to TCP in some scenarios.

Actually the whole idea of QUIC is to circumvent some of the limitations
of using TCP/HTTP2, a lot of stuff was improved on HTTP2 to make it more
efficient, like multiplexing multiple requests on the same connection/stream
and being a binary protocol instead of a text protocol.

But TCP was not designed to multiplex
different streams/requests on the same connection, given one TCP connection
you have one stream and data is sent ordered on that stream, so if you
send multiple requests on the same TCP stream you are subject to ahead of
line issues, where the order guarantee of the TCP stream will delay
the receive of data from a request. Since requests are orthogonal between each
other the order guarantee of the TCP stream hurts performance.

QUIC was born as a way to be more efficient on this model where multiple
requests are made using the same underlying connection, which is reflected
on its design. Here I document some core design decisions that caught my
attention, not much because they are amazing by itself, but they present
considerable differences to TCP and may be a better fit in some scenarios
because of it (the main one being "better HTTP2").


# Built on top of UDP

This one may be accidental, since the only protocols that will work on Internet
infrastructure are TCP and UDP if you want to have a transport protocol that
works on the "wild" you will need to do something on top of one of these two, or
buy a huge fight like IPv6, which so far shows how hard it is to introduce
new protocols at Internet scale. Since networks were designed for encapsulation
it is fairly easy to build new protocols on top of UDP/TCP. Since TCP has
guarantees that goes against the QUIC design, you are left with UDP as an option.

There are two properties that emerge from this decision that I can think so far.
One is that you will always have the option of using your own user space
implementation of QUIC, even when kernels already implement it on the future, which
seems like a nice/useful option that is much harder to do with UDP/TCP.

Another one is that at least for user space implementations you don't have
limitations on how much connection you can establish per process, beyond
the CPU/Memory, since you are going to have just one file descriptor open
for the UDP socket and all connections/streams are built on top of that
on user space. I have no idea how that scales, but I'm kinda curious if it would not
be feasible in that model for a process alone manage 100K connections, something
that is not doable in TCP given the max number of file descriptors limitation.


# Connection identity decoupled from IP

As the time I'm writing this I still don't know much details on how
this work, I just know that QUIC connections are decoupled from IP addresses
in the sense that the identity of a connection is not tied to IP's.

I would guess it is related to TLS and that the enforcement of it on
the protocol guarantees a way to know that after you start a connection
any communication that happens on it is still being done with the same
endpoint, even if its IP address change. So you can decouple from IP
as a mean of identification but without giving up security.

The main property that came to my mind with this is that connections will be
much more stable in mobile networks, where you can change the network you are
multiple times while moving (like in a car) and the connections you made
just keep working. With TCP in a scenario like that you always end up disconnecting
and having to reconnect again, which makes building synchronous request/response
models on top of it annoying, specially if the operation is not cheap/fast.
Building request/response on top of QUIC seems to make it easier to have
simple request/response APIs even when the requests may take some time
to finish, both because you have less disconnect issues and also because
maintaining multiple connections open doesn't hit hard limits like
amount of file descriptors (although you may still run out of memory).


# Always Confidential



# N streams per connection
