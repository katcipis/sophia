# Crypto

I'm not even close to a specialist on any of this, but it is impossible to
not form some opinion since how hyped/saturated the market is on this
topic for a few years. Here I focused specially on the bitcoin model,
including architectural characteristics it exhibits, but there is a lot
of different approaches to digital money, would be hard to write about
all of them.

## Private Money

The idea of de-nationalizing money is intriguing. I just don't think it is
as simple as "no government + private = bliss, fairness, prosperity". It is
very easy to try to solve a problem on a current system without realizing 
new problems you will be introducing on your new system.

On a completely decentralized/private system you could end up with a
cyberpunk neo-feudal system where a few digital oligarchs hold all the
power, even if the underlying tech is decentralized. This is exacerbated
by decentralization/lack of structure. That favors lack of accountability.

## Scalability

This argument is around the idea of having a single public global ledger.
Even for fast operations, having a **SINGLE** replicated data structure
seems damn hard to scale, even if it is append only, but the problem gets
exacerbated considerably if you take into account a proof of work model
as Bitcoin.

If we think about Bitcoin as a currency, imagine that for each person in
the world buying milk in the morning you trigger:

* Write operation on a distributed/replicated data structure
* Consensus needs to be achieved on the network
* Proof of work stuff
* Now you can buy milk

I just have no idea why making every operation in the world global is a good idea.
We should go in a direction where digital can be just as private and
local as actual physical money, a peer to peer operation. And the whole
idea, ironically since it talks a lot about distribution, is based on
global operations on a single ledger.

I just don't see it ever being the cool global money that maybe would be
groundbreaking/life changing. Maybe it can be other things, like store of
value etc, but not an actual currency to use on a day to day basis (and
no El Salvador using it proves very little to nothing, specially because most
places don't use Bitcoin anyway there).

## Tolerance to Partitioning

If you get enough partitioning on the global network on the single ledger
approach you don't have much options other than:

* Stop working (lose availability)
* Fork the ledger, with no way to re conciliate later (AFAIK)

This is a classic problem in distributed systems usually described as the
CAP theorem. Since the ledger uses a quorum you can lose part of the
network and keep working, so it is not like it always ends up with
loss of availability on small partitions. The problem is scenarios like
war that can easily cause a country to be isolated from the global network.
There is no good option for the system to keep working but locally and
then re-join the global network later.

The Internet was designed to work like this (and it does), it is again ironic
that something that is always talking about decentralization failing at
basic things like this.

## Privacy

There is no real privacy on the idea of a global public ledger.
You may have anonymity, but that is based on:

* You keep your own wallet (no exchanges)
* You never leak information that can tie you to your public key

The important thing here is that any tracing or data analysis that can
leak your identity to your PUBLIC key will make ALL your financial
operations public to the whole world. We are not even talking about
leaking/losing your private key, that is not necessary for you to
lose your anonymity.

What makes this extra dumb is that almost everyone uses exchanges, because
maintaining your own wallet means being your own bank, which means
taking security as seriously as a bank. In a world where people barely knows
how computers and the internet works expecting this from people is ludicrous.
So they will end using an exchange and having just as much (or arguably less)
privacy then classic banking (remembering that this landscape is unregulated,
so chances are you have less rights).

## Recentralization

The Internet was designed from the ground up to be extremely decentralized. And it
still is. It was designed to survive half your country being nuked. Literally.
So whatever makes computing today centralized is 100% unrelated to tech.

Just that reality makes me wonder how far building "new" decentralized
tech (with a bunch of problems of its own) will really lead to a decentralized
and egalitarian world. IMHO it just won't, because the forces for centralization
are non-tech related. They are mostly social/economical. So power will end up
being concentrated, the main difference being the lack of accountability
of whoever has this power. 

A lot of the complaints on how big tech took over everything is related to
people not wanting to do shit themselves. We just use facebook, we just use
the cloud. We don't build core tech anymore, we are just clients. Given that
this is the normal human behavior I would expect every human dealing with
the "new" blockchain stuff via services/exchanges, because they just don't want
to do shit, and that leaves us pretty much in the same place we are today, but
with more expensive/complex/mostly useless tech (but new players, which may be
just as bad or even worse than the current ones).
