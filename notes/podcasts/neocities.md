# Bringing GeoCities Back with Kyle Drake

Some notes from the really interesting podcast
[Bringing GeoCities Back with Kyle Drake](https://www.softwaresessions.com/episodes/bringing-geocities-back-with-kyle-drake/).

The core idea around Neocities is to bring back some of the magic of Geocities.
Currently we have a lot of social platforms but they are all very constrained
on the format that you can interact on them, small text box on twitter, short
videos on tik tok, etc. He wanted to provide an easy way for people to have
a blank canvas and be creative about it, like in the early 90's. 

Maybe it is just old people being nostalgic =P, or maybe there is something about the idea that is cool,
but the whole interview is very interesting, specially the challenges on building Neocities
and the implications that has to decentralized web and autonomy.

He talks at length about how bad it is that these days you need to be a true
believer in order to work into something. He likes the idea of a more
decentralized web and yet he is very skeptical about a lot of tech
that tries to achieve that, and even tech he is working on. And people seem
surprised, like how can you work on something that you don't believe it
is going to work 100%, and that is actually healthy, you are rarely a 100%
sure of anything and critical thinking is essential, even with ideas you
like, and for some reason we are losing that, you need to be a "believer"
and an evangelist to be respected.

Then he moves on to some very interesting technical details, like how he ended
up having to deal with BGP and implement his own Anycast infrastructure. Usually
people would react, why do that ? just use CloudFlare, Fastly, etc. And you usually
would be correct and it is what he tried to do, he was using digital ocean to
run Neocities and had some very serious issues.

What happened is that the NRA got pissed off with the content of one of the
pages he was hosting and launched an DMCA against the page, in this case the
ISP would be Digital Ocean and when they received it they did the safest thing
possible, they just brought down all Neocities. In the end the DMCA claim was
not even valid, and yet he was completely powerless to stop his whole service
going down.

That was the start of his quest on achieving independence. He became his own
ISP and then had to pay for traffic for a few internet exchanges. This allowed
him to be the one notified when any legal issue happened and deal with it
appropriately, instead of relying on what a big cloud vendor would do (which
is almost always bringing you system down, because that is safer).

That made me think a lot about true decentralization on the web, and how it will
involve having ownership of much more infrastructure than people are used to, or
else it is fake decentralization/autonomy. Basically if 3 cloud vendors can shut your
whole system down you are not even close of being decentralized, doesn't matter
how much cool voting mechanisms you have or amount of blockchains. In the end
independence, autonomy and decentralization are spectrum's, there is no binary
being centralized vs decentralized, or having no control vs having all control.

Another interesting piece of information is the cost. Cloud vendors charge an
obscene amount of money for traffic and it is con of the century that they
convinced the whole industry that how much they charge for it is OK and makes
sense. His whole setup for being his own ISP + exchange/traffic costs for 30TB
of data is around 130 dollars per month. The same setup on AWS would cost
around 5000 dollars. It doesn't matter how much extra costs AWS have, it is
hard to justify an almost 50x increase on price since the other company also
maintains internet links and does the routing with reasonable high availability.

It makes you think a lot on how keeping things simples could be a differential in
a product these days, you could do the same others are doing but orders of magnitude
cheaper if you can be smart about it. Also on how we keep falling for these
cloud vendor traps.

And talking on traps there is some bonus content on how people seem to have forgot
about Microsoft's past and then just keep using visual studio code all happy.
Microsoft doesn't love open source, they didn't bought Github out of love for
open source, and they don't give you vscode out of the good of their hearths.
There is certainly a board meeting happening where they discuss how they
are going to bring and lock in the dev community inside their platforms and
we just keep following their plan for some reason. And that only gets more
insane with the CoPilot thing, not just the dependence on something provided
by them but specially the ethically questionable way that they got the data
and built the thing (they didn't bought Github out of love for open source...).
