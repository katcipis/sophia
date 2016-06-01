# XP Explained

The greatest part of the book are the values and principles, since they
can be applied on a very great range of ways, and are just focused on human needs.

The book already starts with the notion that removing the human factor of a methodology
or process to develop software does not make sense and it will inevitably fail.

That makes sense to me, since we are all humans.

## Core Values

* Communication
* Feedback
* Simplicity
* Courage
* Respect

Just great :-). And of course courage must be balanced with good sense and the needs of the
team, it is not a hero approach to courage.


## Principles

### Mutual benefit

I really found this to be pretty cool. I always preferred refactoring + clear code + tests
than extensive documentation. And finally I got a good idea on expressing why is that.

When you are just writing a lot of documentation, you are doing something that is only
valuable to other persons (or even yourself) on the future. On the present time, there is no gain
at all on extensive documentation, you already now what the code does, it is boring as hell, because of that.
You are just not getting anything out of it right now.

But if you are writing tests, well, it is challenging, it is hard, it proves that the code you just wrote
works... it is FUN. You got a benefit right now, and later you will have regression tests + good examples
on how to use your API, it is a WIN WIN situation. The same goes for refactoring and well written code,
it just makes you fell warm and fuzzy inside :-) (documentation never does that for me :-().

So, the principle is to always aim at practices that gives benefits right now and on the future, not
just the future...and not just right now.


### Flow

Everything must happens on a flow of continuous value being delivered. Practices like
continuous delivery matches this well. Everything must be developed on small increments that
go end to end. Avoid big bang deployments/deliveries. Value is added on a continuous flow.


## Executive support required

A change like XP requires executive support. After talking about theory of restrictions Kent talks
about cases where after a team successfully applied XP the constraint moved to marketing.

They did not get happy about it and the team got dismantled. XP was set for the blame. So the
change is on the entire company, including executive support.

If there is no executive support you must be ready to do stuff without support and appreciation.
I already did this, it is not awesome but it is better than do a shitty job.

The reward system should also be shift for overall throughput instead of individual productivity.
This way people will feel ok to identify themselves as restrictions and improve on that, instead of
trying to not look as the restriction. Any optimization on the throughput is rewarded.


## Planning


### To point or not to point

I found interesting that after some time I just gave up on the whole point system for
estimations, in favor of just using time (or no estimation at all).
It is advocated that estimations are very important, so does the planning,
and after some time Kent also gave up from points in favor of just plain old time
estimations.

The objective is to have an idea of what **may be** done on the sprint. And time is the most
explicit and honest way to communicate available.

The only requirement is that everyone must understand that it is a plan, with estimates, it is not
a deadline, it is not a precise representation of what will happen on the future, it is a snapshot
of what you think with the knowledge you have now.

In the end I strongly agree. There is no problem with using points too, if you want to. But if
you **NEED** to use points because every time you use time everyone understands it as a deadline them
your organizations have some serious issues (communication, trust, etc), so you really need to address
these problems instead of masking them with points and poker stuff :-).

I prefer to work with only prioritization and getting
shit done. Of course this is not always possible, so it is important to developers to develop some
knowledge on estimation.


### Quality is not a control variable

When talking about how to steer a plan things get interesting. The more traditional/stupid
way to go faster or work with unplanned stuff is to cut down quality.

Quality is not a control variable, if you change it you cant go faster, you are just generating the
illusion of speed since you are generating work that will have to be done on the future, when it wont be
obvious who is the responsible for the problems.

On this point it sinked down in my mind that this is a severe lack of the core values, like courage and
communication.

Instead of being honest and telling that things cant get done on the time frame, requesting a scope change,
you just lie to yourself and to others to give the false illusion that stuff got delivered, when they didnt.

This is the true courage I think, doing this and refusing to deliver shitty/half baked stuff. For a long time I thought
it was courage to change code, technology, architectures, etc. The more I develop software, the less it looks
like it. True courage is harder and is balanced with honesty and respect for the people paying you (if it is
not your company, develop software as if it was with your money).

Delivering half baked stuff will hurt the confidence and respect the client has on you. And internally the respect
and trust of the organization for the team. It takes courage to just say that something can't be done on the time frame
and ask for a scope reduction, true courage.


## Control is an illusion

It is interesting that I believe in that but when it comes to developing software I found
myself panicking because of the lack of control. One of the main consequences of feeling
out of control is working on the weekends, because it really gives you a false sense
of regaining control and kicking some ass.

Dont do that, control is an illusion. If some work will be done on the weekends, be sure this
is not the reason, because it is an stupid reason.


## Testing


Tests creates a safe place to experiment and fail. The dilemma is:
**Defects are expensive. Preventing them is expensive too**.

Again another instance of the problem of finding the nice spot :-).


### DCI (Defect Cost Increase)

One of the few empirical truths about software, the longer it takes to fix a bug,
the more the fix will cost.

As time passes the developer may have changed context, or it may be another developer
that will have to fix it.

So you must test often and soon, so you catch the bug as fast as you can. Writing
the test **before** the actual code is an extreme view of this idea, with the benefit
that helps you think about your API first, decoupled from implementation details
(because you have not implemented it yet).

Also tests are a great way to get feedback about your design. If you write them before you
are getting the feedback as fast as possible, which also seems to be a good idea.


### Double Checking

It is a good idea to have a set of system tests that are written with the customer
perspective in mind. To aid that it is better if these tests are not written directly
by the developer that wrote the other unit/integrated tests. 

Thats why it is double checking, two different sets of minds testing the system.
But the idea is not to test the exact same thing, but to bring a different perspective.


### Manual testing

Not a good idea since it is not a stress reliever, and will actually get worse on stressful
situations. The stress makes you do mistakes, and mistakes on manual testing will push bugs
forward to production.

Automated tests are stress relievers, and they just get cheaper with time (you have more fixtures
and knowledge on how to write them).


## Quick ideas

* Make smaller contracts with clients, both will benefit from it
* Authority must be aligned with responsibility (the one that decides is also responsible for execution)
* TDD the architecture. Want to scale better ? First write the load test that breaks the architecture, them improve it.
* Build a small system and split it as it grows (reminds me of microservices)
* No defect is acceptable, every single one is a way to improve and learn, not part of life.
* Team metrics: Post development defects + Time from investiment to return of the investiment
* Deliver documentation with the system and collect information on how it is used (remove docs that are unused)
