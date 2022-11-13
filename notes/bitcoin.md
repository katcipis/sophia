# Bitcoin

From its inception I never got a strong appeal towards Bitcoin, probably because
I like anarcho-capitalist skills, IDK, but even though I work in tech
and I usually like digital things, there was always something off on it.

I realize now it is probably because of the pattern of someone being naive
and trying to improve things and building tech for that, and then a lot of
people seeing the potential to getting rich without doing much with it
then exploring the idea, in ways that are not as beneficial to people
as they would expect.

There is also the case where some assumptions Bitcoin and people proposing the
change makes that don't make that much sense. You have this whole anarchical
fuck the government vibes, but no clear plan to exactly how you are going to
topple the government, because nothing short of toppling entire governments would
make Bitcoin actually work in a way that is useful.

What would be the useful way ? Well it truly is anonymous and decentralized, no exchanges
involved (AKA Banks) and it is used for everything, buying bread/milk/etc. In a scenario
like that you are expecting people to pay taxes only if they want to, that just
doesn't work, I know it, the government know it, so that shit won't fly anyway
unless you radically change things, and the change is not blockchain, it would
be just a tool if the change itself already happened, but I just don't see it
happening and I don't see anyone with an actual plan.

The internet was born decentralized, the core tech still is decentralized, and most
people are not making the hard question of how it got centralized. I won't delve
on this here, but I will give 2 hints:

* Cloud Computing (AKA Mainframe Computing)
* Not Lack Of BlockChains (AKA a data structure)

In my quest to try to understand how I could be wrong, I went to the source, the
Bitcoin paper, written by the person who no one knows who is, even how Bitcoin
starts is full of enigmas and drama, so exciting :-).

I'm not a specialist on economy/society/etc, but I do like things to make sense,
and honestly a lot doesn't on the paper. Not the math, the data structure, etc.
My overall impression of the person is the classical software engineer thing,
you have someone that is quite smart, knows a lot about logic/data structures/math,
and very little about human beings, how they think, what they want and how society
works.

The [article](https://bitcoin.org/bitcoin.pdf) already starts with a misconception,
it talks about how bad trust is, and how important it is to not require trust
and then goes on how to achieve it:

```
The longest chain not only serves as proof of the sequence of
events witnessed, but proof that it came from the largest pool of CPU power. As
long as a majority of CPU power is controlled by nodes that are not cooperating to
attack the network, they'll generate the longest chain and outpace attackers
```

This is presented as the main way to achieve the requirement to not trust on
a third party. IMHO this is a fallacy and it is the first (of some) core assumption
of Bitcoin that is just plain wrong, CPU power is well distributed across mankind,
so trusting on it means no centralized trust.

That is sadly wrong, we are extremely uneven on our access to computing power and
it is only getting worse thanks to the joy of cloud computing and cloud native
software. More and more CPU power is centralized around a few really huge companies,
meaning that the dream that you should not have to trust them is broken, you will
have to trust them because they could own the network if they wanted (or the
government wanted, etc).

This is an instance of trying to solve a problem without trying to solve the underlying
problem. We are relinquishing more and more computing power every year, in the name
of making it cheaper to build systems, and if we don't stop doing that then
trusting on whoever has more CPU power is a very bad idea.

Well, moving on, then it gets into a no-feature/bug, which is transactions are
non-reversible. This is the perfect example of something that engineers love,
immutable data structures, but that most of the time actual people really don't
want:

```
Transactions that are computationally impractical to reverse would protect sellers
from fraud, and routine escrow mechanisms could easily be implemented to protect buyers.
```

You went completely on the side of protecting buyers, and then there is a mention
of escrow mechanism that easily fix the issue for buyers that doesn't make any
sense with the other properties of Bitcoin.

If you are talking about anonymity, if the systems doesn't provide a way to rollback
a transaction by a mediator, then there is no way to do anything about it, the seller
will just move on.

If there is any sort of identification and then some sort of review system, then
now I can map your wallet/key to an actual ID and I can see all your operations
since the beginning of the times (because the ledger is public), which doesn't
seem like something desirable.

All other alternatives to this end up involving some third party, and then
some misconception on how in this case it will be much better than a classic
third party, when it is essentially the same and there you go having to
trust something.

It is a very human way to structure things with a neutral mediator to solve
disputes, because disputes are very common and they just don't get solved
just by the involved parties most of the time, you can't escape that with tech
because that is a human "feature", there is no amount of tech that can change that.
So you just go in circles and end up on the same place you were before, but with
different players. Or you try to go without it and will have all the problems
people had centuries ago when they didn't had the idea of having some mediation.

Zero trust is an illusion, the best you can do is be very careful about who you trust.

## Proof of Work

On how proof of work works:

```
The proof-of-work also solves the problem of determining representation in majority decision
making. If the majority were based on one-IP-address-one-vote, it could be subverted by anyone
able to allocate many IPs.
```

This kind of problem is really hard to solve in a completely decentralized manner,
I don't have a solution, but I really can't see how that is a solution.
You just moved the problem from "allocating many IPs" to "allocating many CPUs", it
is conceptually the same thing. And specially on public IPv4 addresses I'm not even
sure who is more expensive, so it is debatable if the IP thing would not be
actually better. It is probably a bad idea, but so is the computing power idea.
