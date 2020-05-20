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
