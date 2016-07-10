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
go end to end. Avoid big bang deployments/refactorings.

That is why XP practices revolve around a loop of small improvements on code + design + testing + delivery.
Every step of software development is made at each iteration, and each iteration can be as small as one week.

The idea of flow also impacts the idea of who is responsible for quality. Everyone on the flow of value is
responsible for quality, for its own work, but also to make it clear if some problem is spotted, even if it
is not on the part of the system you are responsible for.

This leaves QA to a possition of double checking and quality assistance, not assurance, since the entire pipeline
is responsible for quality, not one team.

If one of the parts of the flow fails, and value cant be delivered, all the flow fails. It makes perfectly sense.
If one team does a great job and another one drops the ball and the client does not get what he wants, everyone loses.

This incentives to think always about the flow and to think collaboratively, but also raises some really hard questions,
like how you will give bonuses and raises ? Traditional modes of measuring and giving promotions are heavily
focused on the individual, but now you have a model where the value is on the flow, not on the individual. You have
to rethink the entire structure of your company to accommodate the idea and incentive people helping each other.


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

Only manual tests will not work well since they just make stressful situations worse
(more mistakes are made), while automated tests provide the opposite effect, confidence
on the face of pressure.


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


## Incremental design


Incremental design matches well with the Martin Fowlers Refactoring book and with the
Boy Scout principle presented on the Pragmatic Programmer book. Every time you meet some
code, leave it better than it was (but not entirely 100% clean).

Make a habit about changing the design everyday and have patience. The main quality to develop
here is patience, even if you have a vision, if it is too far way, be patient and give incremental
small/stable steps in that direction. Breaking this kind of work is not easy, but it is worth the shot,
the final product will probably be even better than your vision.

The idea is not to not design, is to design a little everyday. Also you can design upfront, actually it
is cheaper to just get the design right first, the problem is that this is too hard since you are
usually resolving a novel problem, that will need a novel solution. The chances of getting the design
all right before starting to solve the problem is really thin (although if it was possible, it would be
cheaper).

So incremental design is better because it is a necessity, you just dont know the solution space
well enough on the start of a project to design it all upfront, but this is a gradient being navigated
based on your experience on the subject. The more you know about it, the more you can design upfront.

This approach also matches well with the principle of constant flow of value (designing does not deliver
workable software).


### Architecture metaphor


There is a good comparison here that shows how dangerous can be to compare software with architecture.
When you are constructing a build, reverting work can change completely in a matter of days, it is not
symmetrical.

For example, fixing a mistake from yesterday can cost 1000$, fixing it 3 days later can cost 10 times more
if you are building a foundation. On software the worst case scenario is always the time spent (symmetrical).
To revert one week work you can just revert one week of changes (and this is the worst case scenario).

So it can be very dangerous to compare these two lines of work. Some concepts can be borrowed but they cant
be compared and practices cant be blindly applied.


## Scaling XP

On this part of the book Kent Beck says a lot of stuff that reminds me a lot of the
current microservices/small team movement.

He presents it as **conquer and divide**. You just solve the problem, and as it starts to get big and
hard to manage, break the problem in smaller ones.

When the team gets too big, you split the teams too, assigning the new smaller problems to each of them.
On this scenario it is important to integrate often (daily) and avoid breakages of APIs (deprecation).

To this to work the teams must be independent and the need of coordination should be an exception
(it will be needed sometimes). To the teams to be independent they must be heterogeneous and have
the capacity to solve problems on a end to end fashion (even business people must be part of the team).


## Organizational Change

Changing an organization is very hard. I didnt get too much good advice on this part as I thought it would be :-).
But the little advice it gives makes perfectly sense.

* If you have a sponsor, be accountable to him
* Start with yourself, its the only thing you have control of
* Lead by example, you cant expect from others stuff that you don't do, this is disrespectful
* If only your team is changing, keep communication as other teams are used too
* Don't push/enforce change to others people, it is not possible. Encouraging through example is possible


## Production models

On the book Kent compares Taylorism with the Toyota production model. On taylorism you have:

* Engineers/smart people that decides how things are going to be done and how much time it will take
* Workers just follow orders (and are responsible for nothing, even quality)
* Quality department assumes the workers are lazy/stupid and "guarantees" quality on the end of the pipeline
* For engineers, quality is another separate department, as sales and marketing

This is the more traditional model, that has been also employed to develop software (since it was what the
industry was more used to). Of course even for cars factories this is not the best model available, enters Toyota:

* Everyone on the production line is responsible for quality, you spot a problem you stop the pipeline
* You stop the pipeline for defects even if the defect is not on your work
* Finding defects is seen as a good thing, and rewarded, you dont have to be afraid of it
* Find the root cause of defects, it is the responsibility of the entire production pipeline
* The whole organization is a quality organization
* Going fast is not giving quality up
* Going fast is not straining
* Going fast is removing **WASTE**

If for producing cars this is better, for software it is even more critical since it is a very creative process.
Removing waste and continuous improvement are the mantra of the Toyota model. Some examples of waste on
the software industry:

* Big architectures to feed people egos
* Useless documentations
* Useless requirements

Basically anything that does not help you deliver something for your client. On the toyota model there is no
pile of parts stocked, it is always lean. In software you should not stock up requirements or documentation for
stuff that you are not even sure if it is going to the client. Take 1 requirement, implement it, deliver it,
get feedback and then repeat.


## Practices

A list of very simple practices, aligned with the principles/values mentioned before.

* Whole team
* Sit together (team)
* Informative workspace (Kanban, for example)
* Energized work (dont strain working too much hours)
* Pair programming (or code reviews)
* Use stories instead of detailed requirements
* Estimate as a way to help business decisions, using time
* Ten minute build
* Continuous Integration
* Test first programming (TDD)
* Incremental design


## Some more ideas

* Make smaller contracts with clients, both will benefit from it
* Authority must be aligned with responsibility (the one that decides is also responsible for execution)
* TDD the architecture. Want to scale better ? First write the load test that breaks the architecture, then improve it.
* Build a small system and split it as it grows (reminds me of microservices)
* No defect is acceptable, every single one is a way to improve and learn, not part of life.
* Team metrics: Post development defects + Time from investiment to return of the investiment
* Deliver documentation with the system and collect information on how it is used (remove docs that are unused)
* Dont push change on people, change your team first
* If just your team is working differently, keep the old communication structure with the rest of the company (dont make enemies)


## The timeless way of building software

This part is inspired on the book [The timeless way of building](https://www.amazon.com/Timeless-Way-Building-Christopher-Alexander/dp/0195024028).
It uses the same idea applied to software. Software has suffered from the same problem as architecture, architects
build stuff to satisfy their own egos, not for people and their needs. In my opinion this happens a lot on software too.

Perfectionist people only think about what they want, and never deliver anything. The "pragmatic" people only wants to
be seem as fast by management, so they deliver crap to the client and bugs are considered normal.

The idea is simple, just think less about yourself and more about who is going to use your software, and that is it.
Very simple and very hard to be done.
