# HTTP 2

Some notes from what I understood from HTTP2.

## Main concepts

* Semantically compatible with HTTP 1 (Headers/Body/Methods/Statuses)
* Multiple streams on same connection (multiplexed)
* Control flow per stream
* Prioritization per stream
* Header compression (hpack)
* Server push (to fix server inlining of resources and reduce round trips)


## Why only HTTPS ?

Here is a catch, why it has to be HTTPS ? Actually, it does not and there is
nothing on the spec that says anything about this.

This is related to seamless protocol negotiation.


### Seamless protocol negotiation

HTTP has space for protocol upgrades, an idea was to use this for HTTP 2.
But there was two problems:

* Latencies, HTTP2 MUST be a lot faster than HTTP or there is no point
* Lots of hops on the network fucks up the protocol upgrade :-)

This already happens with WebSocket. There is a lot of scenarios where
WebSocket works just fine on TLS, and on HTTP it breaks down, with connections
being suddenly dropped.

What happens is expected, when you don't cipher things, people do take a look
at your data :-). There is a lot of old infrastructure that investigates
HTTP traffic trying to detect bad behaviour, when they detect it, they drop
the connection.

When you are on a TLS tunnel no one can see shit :-), so everything works.
Having said that, TLS also has protocol negotiation embedded on it.

So TLS was chosen as a way to negotiate the protocol (HTTP or HTTP2) and
to prevent hops on the network from fucking up your connection.
