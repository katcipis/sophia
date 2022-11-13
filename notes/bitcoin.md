# Bitcoin

From its inception I never got a strong appeal towards Bitcoin, probably because
I lack anarcho-capitalist skills, IDK, but even though I work in tech
and I usually like digital things, there was always something off about it.

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

You went completely to the side of protecting buyers, and then there is a mention
of escrow mechanism that easily fix the issue for buyers that doesn't make any
sense with the other properties of Bitcoin.

If you are talking about anonymity, if the systems doesn't provide a way to rollback
a transaction by a mediator, then there is no way to do anything about it, the seller
will just move on.

If there is any sort of identification and then some sort of review system, then
now I can map your wallet/key to an actual ID and I can see all your operations
since the beginning of the times (because the ledger is public), which doesn't
seem like something desirable.

All other alternatives to this end up involving some third party (escrow), and then
some misconception on how in this case it will be much better than a "classic"
third party, when it is essentially the same and there you go having to
trust some third party (because it is inevitable).

It is a very human way to structure things with a neutral mediator to solve
disputes, because disputes are very common and sometimes (or most of times) they don't get solved
just by the involved parties, you can't escape that with tech
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

And this is another example of conceptually wrong thing on Bitcoin, it is a very sane
approach to avoid abuse to make abuse very costly, I would even say that it is
the default way to avoid abuse on systems. For a public/anonymous system it is
impossible to completely block people from abusing things, but if you make it
hard enough, there is a decent chance you will win. The conceptual problem
here is that to make things hard to attackers you make things harder to everyone,
and that doesn't make a whole sense to me, specially thinking about scaling a
global currency:

```
To compensate for increasing hardware speed and varying interest in running nodes over time,
the proof-of-work difficulty is determined by a moving average targeting an average number of
blocks per hour. If they're generated too fast, the difficulty increases.
```

So thinking about the utopia of decentralized currency and using that in your daily life.
You are free of the yoke of The Government, you have 100% freedom to buy anything
with your bitcoin, everyone is living like that now using it for everything.
Now you have the whole world using this (not even paper anymore, because that is old
and Government owned). How is that going to scale, for the whole world doing
all sorts of VERY essential transactions, like buying food, in a system that is
DESIGNED to get slower/harder the more transactions you have ?

Honestly this is one of the parts of this whole thing that I really don't get how
people get around. The only answer is that Bitcoin will never be used as an actual replacement
for the current bad/classic system, it will be just an option that you use sometimes,
probably to buy drugs or something, because as a truly changing thing that you can
use in a daily basis it just doesn't scale if more than 1% of humanity is using it
(remember that humanity is also always getting bigger).

It is decentralized in a way, but very centralized in the fact that you have a single
global ledger that must be kept consistent, that would already be VERY hard to scale,
but the single global public ledger actually gets slower/harder BY DESIGN. Until
now I never saw any reasoning that could defeat this fact, so for people who believe
in it as a currency and something that will disrupt society, that is very bad news,
most people are going to keep using Mastercard.

When that got obvious, then they started to talk about how it is an asset and protects
you from inflation and other bad shit that happens on the very bad government owned
economy. Well guess what, right now (11.2023) we are going through a terrible
economic crisis and a lot of inflation and Bitcoin just crashed in flames.
Doesn't matter how much you reason around this, the fact is that people who were
expecting it to protect them from inflation/economic crisis just got disappointed.
It may bounce back, who knows, but it seems to be just as speculative and affected
by the economy as other "classic" assets.

# Incentive

The incentive part is also fun because it does an analogy with gold mining:

```
The steady addition of a constant of amount of new coins is analogous to gold miners expending
resources to add gold to circulation. In our case, it is CPU time and electricity that is expended.
```

Well, AFAIK gold mining was not a tool to distribute wealth and it didn't solve
any social injustices/inequality. Why ? Because the ones that benefited the
most from gold mining were the ones already rich enough to have resources to
do the mining in the first place. Most of the gold was not easy to obtain, it
required a lot of resources, explosives, people, etc. So it actually helped
concentrate most of the wealth, since the miners themselves were just employees
and usually exploited.

Now, who again that has access to MOST electricity/CPU ? Individuals ? Or wealth
individuals/organizations ? It his really a change on anything ? Honestly it just
feels like changing the players on the very same game. You will move wealth, but
from some very wealth individuals to others (but no real re-distribution).

Then another wrong assumption on human incentive:

```
The incentive may help encourage nodes to stay honest. If a greedy attacker is able to
assemble more CPU power than all the honest nodes, he would have to choose between using it
to defraud people by stealing back his payments, or using it to generate new coins. He ought to
find it more profitable to play by the rules, such rules that favour him with more new coins than
everyone else combined, than to undermine the system and the validity of his own wealth.
```

The fatal flaw on this argument is that the ONLY reason to attack a network would
be to make direct profit, actually get some coins.

# Payment Verification

Payment verification can be tricky/slow to do since it involves:

```
A user only needs to keep a copy of the block headers of the longest
proof-of-work chain, which he can get by querying
network nodes until he's convinced he has the longest chain, and obtain the Merkle branch
linking the transaction to the block it's timestamped in
```

The specially tricky part lies in "convinced he has the longest chain". This can be so
tricky that for someone actually doing business with it:

```
Businesses that receive frequent payments will probably still want to
run their own nodes for more independent security and quicker verification.
```

I'm sure of very few things in life, but Im quite sure that businesses will
NEVER run their own nodes jut to be able to properly confirm payments, it is
a fact that this will converge to using some kind of service, which puts us
exactly where the internet is today, none of the tech is centralized, but services
and commodification pushes towards centralization. So much for decentralization
and again exacerbates the naiveness on how things actually work and people behave.

# Privacy

Here things also get dicy.

```
The necessity to announce all transactions publicly
precludes this method, but privacy can still be maintained by breaking the flow of information in
another place: by keeping public keys anonymous
```

This already failed, there is while companies dedicated to tracing transactions
and being able to attach some for or identification to keys/wallets.
You are putting all your hopes on a single point of failure, if I'm ever able
to map your ID to the wallet key now I have access to EVERYTHING you ever did
with your wallet, and not just me, EVERYONE in the world, and that with a simple
map of ID -> key.

One way to protect itself from this:

```
As an additional firewall, a new key pair should be used for each transaction to keep them
from being linked to a common owner. Some linking is still unavoidable with multi-input
transactions, which necessarily reveal that their inputs were owned by the same owner. The risk
is that if the owner of a key is revealed, linking could reveal other transactions that belonged to
the same owner.
```

Hmm yeah, today most people that use bitcoin already use an exchange, because it is
a big responsibility to know how to manage securely your own wallet. Imagine
that instead of managing a single key/wallet people are going to manage MULTIPLE
ones. Honestly this is just beyond insane. And anything other than keeping the
wallets yourself involve trusting a third party, like an exchange, the only difference
being that the crypto exchange won't be regulated, so they can fuck you up even more
than a traditional bank if they want. Again this smells a lot like some hard core
hacker with an anarcho capitalist mindset projecting that most people will
behave like he does, building his own servers, maintaining his own things,
fuck the government and the establishment, etc. Empirical evidence is overwhelmingly
towards people not behaving like that at all, and not for lack of options, they
just don't want to be responsible, they don't want liability. People always say
that they want independence, that is intuitive to want, but it always comes attached
with responsibility and liability, and that people don't want.

# Partitioning

Actually there is no section on network partitioning, which seems to me to be a
rookie mistake when you think about distributed systems, network partitions will
happens. Specially on a system built on to of requiring a lot of energy/computing
power I'm quite sure that mining will aggregate on a few clusters.

What happens if the clusters get completely disconnected ? In the event of some
attack/war ?  Do you stop doing any transactions at all (lose availability/
CAP theorem) ? Because if you keep doing transactions now you have a new blockchain,
effectively a new coin, and there is no way by design to re-conciliate the
two blockchains. So once the single system partitions and get into an inconsistent state
it can never be re conciliated again, game over. For a global currency this
seems like a fatal flaw to me and it is completely ignored on the paper. It is like
proposing a design for a distributed system but assuming that network partitioning
just never happens, or not in a way that affects you anyway (like an entire
country getting isolated during a war).

One way to do this is to do exactly what we have today, multiple country based currencies
and you can transfer from one currency to another, you end up with a network of networks,
which is something that works well (basically the Internet), it is in fact more
distributed in the sense that there is no single global anything, you have mesh
network of multiple things that are independent but can cooperate too. Bitcoin is
not that and the paper seems to just ignore the issue altogether.
