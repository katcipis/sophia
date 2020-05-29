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
the receive of data from a request.

QUIC was borne as a way to be more efficient on this model where multiple
requests are made using the same underlying connection, which is reflected
on its design. Here I document some core design decisions that caught my
attention, not much because they are amazing by itself, but they present
considerable differences to TCP and may be a better fit in some scenarios
because of it.

# Built on top of UDP

# Always Confidential

# Connection identity decoupled from IP

# N streams per connection
