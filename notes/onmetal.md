# On The Metal

[On The Metal](https://oxide.computer/podcast/) is a podcast from
the Oxide Computer Company. It is the only podcast so far that I know
where each of the interviews is amazing, there is no boring/bad interviews,
they are all filled with great stories and it is specially interesting with
people who has interest on low level stuff (although not entirely focused
on that). Here is some notes on the most interesting interviews.

# Ron Minnich

Ron is know to me as a guy who tries to build on top of a lot of
nice ideas from Plan9 and works on the [HarveyOS](https://github.com/Harvey-OS/harvey).

## Constraints

There was a lot of talk on the role of constraints and good design,
there is almost a trend for me on how good design usually emerges
in environments where resources are scarce, it is like the scarcity
pushes you towards ingenuity and simplicity. It really pisses me off
how web and cloud environments usually foster wasteful/bad designs
filled with inefficiency, and this is not about using assembly because
high level languages are slow or anything, overall wasteful bad designs.

Perhaps for historic reasons the first thing that comes to my mind is
the whole service mesh thing, independent from the benefits obtained it
seems that adding an entire user space layer + doubling the amount of
processes you run on a distributed system does not seem to be the most
efficient way to get resiliency and metrics (and the other benefits).
But I could be wrong, I'm not that knowledgeable with distributed
systems.

## Control

I was always very intrigued on why Plan9/Inferno seemed to be ignored
by most people, specially since they were built by the same people
who built Unix, but improved for distributed systems, which is a perfect
fit for the world we live today (Unix is not).

It seems that one big reason for that happening is human nature, and its
desire for control (which is an illusion IMHO). I was already aware that
one of the reasons neither Plan9 or Inferno were embraced was licensing,
but on this interview Ron talks about the reasoning behind how they
licensed Plan9 and Inferno.

It was the opinion of a lot of people involved on Unix that it was a failure
because AT&T "lost control" of the project, people just took it and used
how they wanted, changed it, etc. Instead of observing how they were able
to influence/mold modern computing, they just felt as a failure because
they where not "in control". And that is the reason why they did not wanted
to make the "same mistake" with Plan9/Inferno, and that is why both were
never truly adopted, a lot of very good ideas never made into the current
operational systems, just now some of them are making it but very slowly,
all because of this fascination that humans have with illusions such as
control.

## Complexity

The holy grail of computing is to get rid of unnecessary complexity and
only keep inherent complexity, the one that is determined by the problem
being solved. But in the industry it is very common for people to believe
that complexity is endemic, mistaking inherent complexity with unnecessary
complexity. An example of that mentioned on the interview is how firmware
was developed.

Ron has seen firmware being built with languages even more
high level than C, but for some stupid reason the entire industry was convinced
that firmwares NEED to be programmed in assembly, like it was a necessary
complexity for firmwares, and that for no good reason, just industry/group
thinking and lack of imagination. It is funny because Ron actually saw firmware
being developed on high level languages, so it was not a subjective discussion,
it was possible because it had been done, and yet people insisted with no
good reason that it was not.

Perhaps the whole service mesh thing is the same instance of this issue,
adopting unnecessary complexity as necessary and dying holding it on your
arms and swearing that there is no other way to achieve resiliency on
distributed systems.

## Predictions

They talk a little about the fallacies of predicting the future too.
He gives some examples of "awesome" predictions he has saw on his life:

* The future is IBM mainframes
* The future is Windows
* Linux widely regarded as a toy

Since human nature/abilities doesn't change very fast, keep that in mind
when you hear people on the industry making predictions about the future.

I prefer Alan Kay's idea of inventing the future as a way to predict it or
Paul Grahams idea of being flexible and adaptable instead of putting energy
on predicting, because predictions almost invariably fail.

## Abstractions

They talk how the right simple abstraction is fundamental to tame
complexity. One of the reasons of Plan9 awesomeness was how far
they were able to push the file abstraction as a means to export/consume
services in a very simple way. It does not fit everything (that would be
the holy grail), but they took it much more far than Unix in a way
that made distributed computing much easier.

The core lesson is the importance of these few good abstractions and that
convincing yourself that you found them is not a very good idea, we still have
a lot to learn on how to come up with good abstractions for
distributed computing.
