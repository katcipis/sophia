# DevOps

It is about time for me to start writing a little about my whole experience with DevOps/Platforms.
Since Im quite curious and I like to know how things works, I was always the guy who even though identified
myself more with software engineering in general (I love coding/algorithms/design/architecture/anything software) I
was also the guy who would have some SysAdmin skills, like actually knowing how networks/switcher/hubs work.
Working at a Telecom company as my first job probably helped :-).

It is really hard to generalize things, so I will start first with what went wrong with DevOps and the
cost of it going wrong, specially to startups that want to be cost effective and move fast.
In the context of a bigger well established company, a lot of the rationales of big platforms (specially internal ones) 
and DevOps/SRE teams can make some sense (just as bigger companies will just keep being slower and less inovative too,
you are playing a different game, scaling good engineering is hard).

## What went wrong ?

Well what went wrong is pretty simple. DevOps (the actual name) started as a cultural movement.
Operations/SysAdmins were pretty distanced from Software Engineers. People writing the code knew nothing
about how things were going to be run and maintained in production and the people running things in production
knew very little about how the software was written and what it does.

The idea of DevOps was to merge both as much as possible, instead of having more specific roles software engineers
would have to be responsible for more and understand better how things work. This ressonated well with Cloud Computing
since now infrastructure started being defined also with software, so managing infrastructure was just more software.

By putting it all together the idea was to avoid the issues of overly complex software that is hard to configure and scale.
If the people responsible for configuring/deploying/scale software are the same people who wrote the software you can bet
it will be easy to configure/deploy/scale, people are quite lazy. Software is usually a nightmare to operate because the
people writing it don't have to operate it, it is abstracted from them.

This is a lesson in how abstractions can go wrong.

The first wave of abstraction was people, you had entire teams doing the work for you, the second wave is the natural one,
lets automate that too, but the abstraction remains and programmers remain ignorant. That second wave incorporates things
like Serverless and Platform Engineering/IDP (Internal Developer Platforms).

It is not easy to argue about this topic because it is not simple. Of course good tooling is good and avoiding repetitive tasks
and simplifying workflows is good. In that sense the case for platforms is made and it is a winner. The problem is how it
is usually accompanied with deep ignorance and applying in contexts that don't require it (small scale).

Now that I think about it... it is the usual problem with all abstractions,
people tend to rely way too much on the abstraction to the point that they become completely ignorant about how it works
and become helpless. And I have seen this happen so often on startups that I feel compelled to write a little about this,
jot down some ideas.

The main problems with abstractions are general ones, the same happens with infrastructure abstractions,
abstractions break on these situations:

* Leaks (the abstractions fails to abstract details)
* Broken (abstraction is not working well...what now ?)
* Performance (things are very slow, there is no button that magically makes it fast...what now ?)
* Efficiency (everything is great, but your Cloud Costs are around 10K per month for something that could easily be 1K)

All the aforementioned problems will require someone that can open the abstraction and understand how it works underneat.
This is not a huge problem for well estabilshed/stable tech, I don't know how to maintain a car, it is mostly abstracted
to me, but it is something much more stable/static. With software the main problem is that we don't know what we are doing.

![](https://github.com/katcipis/memes/blob/master/no-idea-what-doing.jpg?raw=true)

So when people promisse you amazing/cost effective abstraction there is a decent chance that they are wrong and you
are going to pay the price if you have no way to assess the abstraction and move away from it if it doesn't works.

Pressure to have more and more software engineers exacerbates the problem. We have a field with more and more people who
have no idea how any of this works just relying on abstractions that supposedly will take care of everything but it won't.
And that sums up pretty well my own experience, I have been fixing/improving this kind of thing for more than 8 years at
this point, it still is pretty much the same, the difference is maybe that now you try to explicitely build and constrain
people with a platform, but still the price of ignorance on how things work is still paid, because as mentioned before, engineers
who have deployment/infrastructure abstracted from them won't be able to help you improve things with the knowledge they
have of the system they are building (they can't make the connection).

Another side effect of ignorance + Cloud Magic Elasticity is terribly inneficient software. And this one is a very modern phenomenon.
On the beginning of the web/internet you had to be efficient because there wasn't infinite money lying around, even though
the web can be quite elastic in behavior there was a very clear constraint, money. Hardware (and the Cloud) costs money, the
less you spend more chances you have to survive, unless you just get millions of investment and then no one cares.

From [The Other Road Ahead](http://www.paulgraham.com/road.html):

```
Efficiency matters for server-based software, because you're paying for the hardware.
The number of users you can support per server is the divisor of your capital cost,
so if you can make your software very efficient you can undersell competitors and still make a profit.

At Viaweb we got the capital cost per user down to about $5.
It would be less now, probably less than the cost of sending them the first month's bill.
Hardware is free now, if your software is reasonably efficient.
```

Paul Graham's essay is not about this, but it is what triggered me writing this. From all the predictions made on the
essay about the future of the web, this is the one that failed the hardest. We went 100% on the direction of efficiency
doesn't matters and "hardware is cheap" instead of leveraging the web to build things are almost free (since they would
cost very little to run).

It may be cheap, but 30$ to run something that could be run with 3$ still is
a huge difference, specially at scale. And yet this is really completely lost in a way that is mostly depressing.
And the culprit is way too much money, our industry has way too much venture capital and an insane demand for more and
more software engineers, so we have people who knows less and less and barely knows how anything works doing things,
just following trends from Cloud Vendors (who want you to spend money by the way) or copy pasting (or generated
by LLMs) things that do work but are terribly inneficient.

This excessive/premature abstraction of the Cloud combined with huge amounts of venture capital created the problem we
see today. One of the most well payed jobs right now (2023) is "DevOps Engineer" or "Platform Engineer", becase we put ourselves
on this position, we have more and more infrastructure that is insanely complex and expensive because of the current approach.
Another thing that makes the problem even worse is that the promisse of the Cloud to make things simpler didn't materialized.
Some things are indeed easier, like managing a fleet of VMs to run thousands of things, and yet it is still now simple.
Don't trust me ? try to run GKE and solve in an end to end manner the following basic problems:

* Automate creating/managing the cluster
* Manage IAM to give permissions for services running on the cluster to access other cloud resources (like databases)
* Secret management for your workloads inside the cluster

There is a plethora of services/solutions that range from paid/closed source to free/open source. In the end it is not simple,
or maybe I'm stupid, but I never saw a truly cheap/simple solution to this, and that is just the very start of your
problems in the Cloud. So in the end, even though the Cloud can be quite useful it didn't deliver on the promisse of
just making everything easier/simpler, I have spent countless hours going through Cloud vendors documentation, definitely not simple :-).

So in the end what went wrong is that we were never able to really bridge the gap between ops and dev... in general ops people (called DevOps
because they know how to "code" in bash/HCL/YAML) still are just as ignorant about overall software engineering and services as dev people
are ignorant about infrastructure/tooling/scaling.

There are a few people who are truly DevOps in the send that they know how their stuff is run, but I don't think the ratio changed,
so in general the movement seems like failure if the objective was a cultural change. Lot of the tooling to automate things,
like Terraform/Containers, can be seen as a form of win and it came from the whole DevOps movement in a sense, but it doesn't count
because better tooling is just a side effect, the objective was to change culture and I don't think the culture changed at all.

Recently I was able to work with younger people and it consolidate my view that very little changed. You can easily see the failure of DevOps
when you compare with automated testing in general. Today even though people still write terrible tests it is common knowledge that the
programmer writing the code should also write the tests for it, even if he does it poorly, it is clearly his/her responsibility.
This never truly materialized in the industry for operation, still most companies, even startups, have engineers who don't take care
of operating their own services and that is still seen as "OK" because they are too busy with the software (this was the same excuse
for the testing and why you needed QA). So in the end I don't think anything changed much, maybe a little ? The trend seems to be
more and more high level systems that do the deployment for you (serverless), which makes it hard for software engineers to develop
mechanical sympathy and care about efficiency, or maybe it is the other way around, we have these systems exactly because most
software engineers don't care about any of this.

I have seen people freash out of college (after studying computer science for 4 years) who didn't had even the most basic understanding
about how containers work and this is the current fundamental building block for deploying software. That did make me pause too and observe
how we didn't progressed much (or at all) in the whole merging dev and ops thing.

This may be OKish for medium sized/successful companies who can tolerate average performance, but I do believe it is a problem for startups.
Most startups just "survive" for a few years because they are stupidly well funded. The only way to have a really small team and be efficient
as hell (the holy grail, 10x engineering) is doing what is described in [The Other Road Ahead](http://www.paulgraham.com/road.html):

```
Viaweb was written by just three people. I was always under pressure to hire more, because we wanted to get bought,
and we knew that buyers would have a hard time paying a high price for a company with only three programmers.
(Solution: we hired more, but created new projects for them.)

When you can write software with fewer programmers, it saves you more than money.
As Fred Brooks pointed out in The Mythical Man-Month, adding people to a project tends to slow it down.
The number of possible connections between developers grows exponentially with the size of the group.
The larger the group, the more time they'll spend in meetings negotiating how their software will work together,
and the more bugs they'll get from unforeseen interactions.

Fortunately, this process also works in reverse: as groups get smaller, software development gets
exponentially more efficient. I can't remember the programmers at Viaweb ever having an actual meeting.
We never had more to say at any one time than we could say as we were walking to lunch.

If there is a downside here, it is that all the programmers have to be to some degree system administrators as well.
When you're hosting software, someone has to be watching the servers, and in practice the only people who can do
this properly are the ones who wrote the software.

At Viaweb our system had so many components and changed so frequently that there was no definite border between
software and infrastructure. Arbitrarily declaring such a border would have constrained our design choices.
And so although we were constantly hoping that one day ("in a couple months") everything would be stable enough
that we could hire someone whose job was just to worry about the servers, it never happened.
```

It is interesting that he seems this as a disadvantage, a downside, and I get it because it is more work and more responsibility.
But it is what makes you design good and efficient systems and allow you to move insanely fast, the kind of speed that an
average company never will be able to have. And he makes it clear why, there is no definite border between software and infrastructure.
We are trying to define it, so we can specialize people and make it cheaper, but I think it is too soon, we still don't know
where the border really is and what are the abstractions/interfaces, all this is way too new.

You end up needing people who are harder to find and may be more expensive but they will be able to deliver exponentially more for a
fragment of the infrastructure cost that an army of software engineers powered by a DevOps/Platform team would.
