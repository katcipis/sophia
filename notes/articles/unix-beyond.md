# Unix and Beyond: An Interview With Ken Thompson

Notes from [this](http://cse.unl.edu/~witty/class/csce351/howto/ken_thompson.pdf) awesome interview.


## Good simple abstraction

When asked about this Ken asnwers that he develop small and simple blocks
because he has a very bottom/up way of thinking and he just
can't build complex systems otherwise:

```
When I see a top-down description of a system or language that has
infinite libraries described by layers and layers, all I just see
is a morass. I can’t get a feel for it. I can’t understand
how the pieces fit; I can’t understand something presented
to me that’s very complex. Maybe I do what I
do because if I built anything more complicated, I
couldn’t understand it. I really must break it down into
little pieces.
```
I'm not even sure if I'm more bottom/up or top/down, but doing things
just because you need to understand it, not for some quest for purity,
is a feeling that I can relate too. I believe great part of the refactorings
that I do on systems are based on this same principle, I usually:

* Start reading the code
* Can't understand it properly
* Finally got it
* Analyze what was missing to make me understand it faster
* Fix it

This is a complicated matter, because what allow me to understand
code better may not be exactly what will help someone else. Design
and cognition is hard x_x. But I found that it is a pretty cool way
to refactor things, instead of just deciding that code should be
better and you **MUST** refactor it, it is kind of a "on demand"
refactoring =P.

I like to think about it as reducing cognitive distance, it is the
distance between where you are and where you wanna be. In this case
where you wanna be is understanding what the code does, where you are
depends on how familiar you are with the problem (hence the distance
varies greatly).


## Communication

It is interesting that when asked about which tought process is better,
bottom/up or top/down, he says this:

```
I think there’s room for both,but it makes for some interesting
conversations, where two people think they are talking to each other but
they’re not.

They just miss, like two ships in the night, except that
they are using words, and the words mean different things to both sides.
I don’t know how to answer that,really. It takes both; it takes all kinds. 
```

I have found myself a lot of times on the same situation where I'm trying
to explain something, and the discussion won't evolve gracefully, and just
after sometime that I realize that we were using the same words but
thinking on different means to them. It is a reminder of how communication
between people is hard.


## Open Source

Almost everything (if not everything) in life presents a tradeoff.
In the case of open source it is hard to see the downside of allowing
all people from all cultures and background working together and
collaborating to solve a common problem. The more young and naive intuition
it that this is always great, specially because we mix the problem
of developing software and good abstractions with social problems
that are not related with how to build systems at all.

When asked about the reason for such simple abstractions:

```
The aggressive use of a small number of
abstractions is, I think, the direct result of a very small
number of people who interact closely during the
implementation. It’s not a committee where everyoneis trying to
introduce their favorite thing.

Essentially,if you have a technical argument or question, you have
to sway two or three other people who are very savvy.

They know what is going on, and you can’t put anything over on them. 
```

And it not just the case of Unix/Plan9/Inferno. A lot of other good
technologies are built by a small group of people, like Lua and Go.
Lua is a good example where some people find the community bad, because
they don't listen to people, don't "improve" the language. The counterpart
of that is Python, where everyone is heard, loved and huged and the language
only grows and grows like a cancer. Python Zen talks about of having just
one way to do stuff but that is laughable at the point where Python is now.

When all projects accepts everyone suggestions they all converge to the same
thing. In the end no project has any culture or personality or vision whatsoever,
they are just a blend of everyones idea again, with some spin. Trying to please
everyone will make you end with something that is not ideal to anyone, it is
like a common denominator, it is a mediocre tool for everyone, instead of being
a **VERY** cool tool for some people and a bad one for others (depending on context).

Since we are humans and the way we develop software is influenced by our
feelings and beliefs, this is one of the feelings and cultural changes
that seems to hurt more than bring benefit. Anyone should be welcomed to a project,
but if a suggestion is not implemented it does not mean that the person is
not heard.

Another obvious problem is that consensus among human beings is extremelly hard
to achieve. One of the models applied when developing Go in its early days to
stop the language from having a lot of features was that any feature would
only be inserted on the language if there was unanimous consensus that it
was a good idea (avoiding everyone inserting its favorite stuff). On the
case the team had like four or five people, and it worked great. Try to
do that with 100 people, or even 10.

Since the tradeoff is always there, perhaps this massive open source approach
probably brings some benefits too, perhaps social instead of technical, but
it sads me that no one sees the downside, like it is a communist panacea.


## Managing Creative Processes

On how was management at Bell Labs he says this:

```
As for the process, it’s hard to describe. It’s chaotic,
but somehow something comes out of it. There is astructure that comes out of it.
I am a member of the Computing Sciences Research Center, which consists of a bunch
of individuals — no teams, no leaders.
It’s the old Bell Labs model of research; these people just interact every day.

At different times you have nothing to do. You’vestopped working for some reason—you finished a 
project or got tired of it—and you sit around and look
for something to do. You latch on to somebody else, almost like water molecules interacting.
```
TODO

## Conflict Resolution