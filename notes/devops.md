# DevOps

It is about time for me to start writing a little about my whole experience with DevOps/Platforms.
Since Im quite curious and I like to know how things works, I was always the guy who even though identified
myself more with software engineering in general (I love coding/algorithms/design/architecture/anything software) I
was also the guy who would have some SysAdmin skills, like actually knowing how networks/switcher/hubs work.
Working at a Telecom company as my first job probably helped :-).

It is really hard to generalize things, so I will start first with what went wrong with DevOps and the
cost of it going wrong, specially to startups that want to be cost effective and move fast.

## What went wrong ?

Well what went wrong is pretty simple. DevOps (the actual name) started as a cultural shift movement.
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

This is a lesson in how abstractions can be wrong.

The first wave of abstraction was people, you had entire teams doing the work for you, the second wave is the natural one,
lets automate that too, but the abstraction remains and programmers remain ignorant. That second wave incorporates things
like Serverless and Platform Engineering/IDP (Internal Developer Platforms).
