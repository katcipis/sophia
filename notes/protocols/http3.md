# HTTP 3

One of the main reasoning's of http3 is some shortcomings of TCP,
in special the limitations of multiplexing multiple streams
on top of a TCP stream. TCP was not built with that in mind,
so you end up having a head of line issues, which is caused
by TCP one stream model + guaranteed packet order. The idea is not
that TCP is terrible because of that, when you want just one
stream it is pretty good, but when you want multiple streams you
end up creating a lot of connections, like http 1, or multiplexing
N streams on top of one stream, like http 2, which is also
not optimal because of head of line issues.

Given that one of the main limitations of http2 is because of TCP limitations,
one of the most interesting artifacts built from HTTP3 is the QUIC
protocol itself, which provides abstractions similar to TCP but with
some different key design decisions. It is specially interesting because
QUIC has been developed as a separate transport protocol, on top of UDP,
so you can use it also for non-http3 traffic. Basically you have a new option
besides TCP/UDP to do networking, that will work on the internet infrastructure
(or http3 will never exist).

The core differences is that a QUIC connection is built on top of UDP
not IP, which brings tradeoffs, and an interesting one is that the
concept of a connection is not tied to specific IPs/Port anymore, so
you could be on a mobile connection, change your IP, and keep all
your connections. Maintaining open connections gets pretty inexpensive,
there is no open file descriptor per connection, no reconnection issues
(at least no obvious ones). Since this brings some security concerns
(you don't have the IP as a form of identifying yourself), QUIC obligates
the use of TLS, which is how identification is performed (instead of using IP),
or else it would be trivial to hijack a QUIC connection.

So far I only see some interesting advantages on the QUIC level, like lightweight
connections, faster handshake for establishing new connections, etc. I'm not
aware of anything terribly different on http3 itself, besides being built
on top of QUIC instead of TCP.

As a whole http3 would turn much easier to implement basic request/response
models even for requests that may take a long time to produce an answer,
avoiding the complexity of asynchronous APIs.


# Material

* [HTTP 3 for everyone](https://fosdem.org/2020/schedule/event/http3/)
