# Coders at Work

These notes are based on my reading of the book
[Coders at Work](http://www.codersatwork.com/).

The first impression that I get from the preface of the book is how
young our field really is. There is an entire generation of people alive
that was born on a world where there was no computers and we are essentially
very near of the first generation of programmers.

For a field that has impact in human life as computing have this is very thrilling,
being part of the beggining of something so awesome. It reminds me of
[The Humble Programmer](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html):

```
The point was that I was supposed to study theoretical physics at
the University of Leiden simultaneously, and as I found the two
activities harder and harder to combine, I had to make up my mind,
either to stop programming and become a real, respectable theoretical
physicist, or to carry my study of physics to a formal completion only,
with a minimum of effort, and to become....., yes what? A programmer?

But was that a respectable profession? For after all, what was programming?
Where was the sound body of knowledge that could support it as an
intellectually respectable discipline? I remember quite vividly how I
envied my hardware colleagues, who, when asked about their professional competence,
could at least point out that they knew everything about vacuum tubes,
amplifiers and the rest, whereas I felt that, when faced with that question,
I would stand empty-handed. Full of misgivings I knocked on van Wijngaarden’s
office door, asking him whether I could “speak to him for a moment”;
when I left his office a number of hours later, I was another person.

For after having listened to my problems patiently, he agreed that up
till that moment there was not much of a programming discipline, but then
he went on to explain quietly that automatic computers were here to stay,
that we were just at the beginning and could not I be one of the persons
called to make programming a respectable discipline in the years to come?

This was a turning point in my life and I completed my study of physics
formally as quickly as I could. One moral of the above story is, of course,
that we must be very careful when we give advice to younger people;sometimes they follow it!
```

Which happened just a few decades ago, and contrary to some beliefs not a lot of
things have changed, we progressed but we are far from being well consolidated.

The book has interviews with a lot of people that have developed world changing
software, and the view that they have about software and its design varies greatly,
this is a sign to me that we are still on a phase of exploration in computer science.

It is important to understand that because human beings do not handle well uncertainty
and we are prone to findings truths and preaching about our truths, quoting
[SICP](https://mitpress.mit.edu/sites/default/files/sicp/index.html):

```
This book is dedicated, in respect and admiration, to the spirit that lives in the computer.

``I think that it's extraordinarily important that we in computer science keep fun
in computing. When it started out, it was an awful lot of fun. 
Of course, the paying customers got shafted every now and then,
and after a while we began to take their complaints seriously.

We began to feel as if we really were responsible for the successful,
error-free perfect use of these machines. I don't think we are.
I think we're responsible for stretching them, setting them off in new
directions, and keeping fun in the house.

I hope the field of computer science never loses its sense of fun.
Above all, I hope we don't become missionaries. Don't feel as if
you're Bible salesmen. The world has too many of those already.

What you know about computing other people will learn.
Don't feel as if the key to successful computing is only in your hands.
What's in your hands, I think and hope, is intelligence: the ability
to see the machine as more than when you were first led up to it,
that you can make it more.''

Alan J. Perlis (April 1, 1922-February 7, 1990)
```

Even if the **fun** has no appeal to you and you are more interested on the
business side of computing, understanding that we are still not on the point
of being sure on how software must be done can give you an edge that very
few people have (at least in my empirical observation of the world).

This is the starting feeling of reading the introduction of the book and it
is a very good feeling. The book is neatily divided in different interviews
so it makes sense to divide like this here.

## Jamie Zawinski

I did not know Jamie and his history is very interesting. He is one of the
few people (that I now know) that had contact with lisp very early on its career.
This gives him a very interesting perspective of things.

Being used to the high level concepts of lisp he is not very found of C for example.
It seems that C for him is just a necessary evil to do systems programming. He does
not say that explicitely, but how he talks about how important language supported
memory management is and how debugging low level stuff with gdb is hard I kinda get
the feeling that he would always choose a higher level language if possible. It makes
sense to me.

He has a very interesting history working at Netscape, he talks abour working 16 hours
a day, how that was necessary to develop a working browser in six months and also
how this caused a burn out in him that made him eventually quit from the software industry.

It was not just the working like crazy, but he explicitely mentioned that it was a big
factor. It is not sustainable, but he believe that in the case of netscape it was necessary.

Sadly Netscape bought a company and gave control of development entirely to this company,
which never had a successful product. They rewrited almost everything in C++ (it was C)
and basically killed the product. The disdain that Jamie has of C++ is remarkable, and
it is for the most traditional reasons ever:

```
o I really tried to avoid using that as
much as I could and do everything in C at Netscape.

Which was pretty easy because we were targeting pretty small
machines that didn’t run
C++ programs well because C++ tends to bloat like crazy as soon as
you start using any libraries. Plus the C++ compilers were all in flux—
there were lots of incompatibility problems. So we just settled on
ANSI C from the beginning and that served us pretty well.
```

How things where redesigned to get bloated:

```
They thought just by virtue of being here, they were bound for glory
doing it their way. But when they were doing it their way, at their
company, they failed. So when the people who had been successful
said to them, “Look, really, don’t use C++; don’t use threads,” they
said, “What are you talking about? You don’t know anything.”

Well, it was decisions like not using C++ and not using threads that
made us ship the product on time.
```

Which shows a fatal flaw of ignoring what the previous system does right.
Them there is more classical computers science errors on the way:

```
They didn’t start from scratch with a blank disk but they
eventually replaced every line of code. And they used C++ from the
beginning. Which I fought against so hard and, dammit, I was right. It
bloated everything; it introduced all these compatibility problems
because when you’re programming C++ no one can ever agree on
which ten percent of the language is safe to use. There’s going to be
one guy who decides, “I have to used templates.” And then you
discover that there are no two compilers that implement templates
the same way.
```

And about other mistake that caused another product to fail:

```
There was just this big blank rectangle in the middle of the window where
we could only display plain text. They were being extremely academic
about their project. They were trying to approach it from the
DOM/DTD side of things. “Oh, well, what we need to do is add
another abstraction layer here and have a delegate for this delegate
for this delegate. And eventually a character will show up on the
screen.”
```

It is sad on how we fail to learn from our past and still pursue these
kind of silver bullets and "right" ways to model things, even when all
evidence shows that it is not helping.

His approach to software design is pretty evolutive, design is an ongoing
process that just ends when the software is "done" (perhaps shipped ?).
He has a very prototypical mindset, where having a small version of the
software up and running is essential to guide the rest of the development.

The most important thing for him is to get something working fast and build
from that. It is pretty GUI oriented in the sense that he always developed
software with graphical interfaces and always used that as a guide to what
are the next steps, literally looking at buttons and interacting with the
interface to think on the next steps.

It is a very common way to develop software among lisp programmers, at least
it is what I think (not the GUI part, but the experimentation and fast iterations).
I read something really similar before on the Hackers and Painters book.


## Brad Fitzpatrick

Brad has a very interesting experience in the sense that he started to
code with 5 years old and yet he got though collegue studying CS. Even though
he already knew a lot about coding the more formal CS education allowed him
to develop a better vocabulary and to communicate ideas easier to other people.

He is one of the first interviewee that places a lot of importance on unit testing.
He requires it from people that works with him and uses testing a tool
to debug and to explore the behavior of libraries.

One thing that got my attention and made me feel less stupid is that his
"hard bug" history is not a bug at all, it is about when he spent like 90
minutes trying to debug why a file was not being changed by the program
he was debugging just to discover that the filepath was actually wrong
and that was the reason the file was being left untouched by the program =P.

My default mode when debugging is always thinking that the problem is something
really complex, and almost all the times is some stupid/silly mistake or
not even a bug (a wrong parameter, wrong port, etc). Keeping your calm and
checking out stupid obvious stuff before debugging can be very hard.


## Douglas Crackford

In his interview it is clear that he is a guy that places a lot of importance
on communication, mentioning that it is even more important than math
for a programmer. Of course that this depends on context, but it is
true that almost all problems in software development in the industry
is caused by communication problems (or the lack of).

He defines himself as a writer more than a programmer, specially because
the programmer role usually involves writing docs, specs and other forms
of prose that are as essential as the code. Code usually is placed in high
regard because it is the executable artifact, it is what will produce the 
results that you can sell, but you seldom will be able to maintain it
properly without a great deal of writing (docs/specs).

Another thing that I was able to relate with him is that he gets sad
on how people are usually not very interested in the history of computing
and what are the origins of the ideas that they are using today. If you
dont know the past you are faded to commit the same mistakes over and over again.


## Brendan Eich

The first thing that got me by surprise since I don't like Javascript is how
much I liked Brendan Eich, which is a lesson on how stupid is to connect one
thing to the other x_x.

I identify myself with him a lot. He really apreciates good stuff like Scheme
and Smalltalk, and he is so against traditional OO and Java and design patterns
that he made a lot of effort to not allow things like classes in Javascript
(sadly it got in lately =().

But as it is usual with software there are a whole context that people tend to
ignore because they where not there, and he explains a little the forces that
where present at Javascript conception. One obvious one, even on the name of
the language, is that the syntax should resemble Java, for the obvious reason
of allowing Java programmers to hack stuff on it fast. So he admits that even
thought we would be best served with a Scheme, at the time it did not seem
practical given the context and the need for adoption of the idea.

Another well know force was pressure (and programmers really shine under pressure =P).
The language had to be done in less than two weeks, in the end it took 10 days.
In that time frame he tried to get what seemed cool from Scheme and other languages
like Self, but it was really hard to understand what features would compose well
or be useful given the context of the browser and the people that would code on it.

A lot of good stuff got into Javascript actually, specially lambda support (much better than
Python for example). If you use the right subset of Javascript it resembles Lua a lot, which
is a language that I really like. So if you squint you can see this core at Javascript
that is inspired on good ideas, but then you get pressure to go faster and faster, you
get standards and commitees, multiple different implementations, bug's that got so
common spread that are required to be maintained on the specification that needs to be
kept to avoid breaking the web.

But in the end he really seems like a smart and reasonable guy, he is very interested
on theory and language design but at the same time gives a lot of importance to
being practical.

Another example is his take on design patterns which resembles me of Paul Grahan:

```
I have this big allergy to ivory-tower design and design patterns. Peter
Norvig, when he was at Harlequin, he did this paper about how design
patterns are really just flaws in your programming language. Get a better
programming language. He’s absolutely right. Worshipping patterns and
thinking about, “Oh, I’ll use the X pattern.”
```

And to backup this notion of the harm that mindless application of patterns
can cause he has his experience at Netscape where a big rewrite fueled by
design patterns ideas and extreme classical OO went extremelly wrong and broke
the company:

```
There was an imperative from Netscape to make the acquisition that
waved the Design Patterns book around feel like they were winners by using
their new rendering engine, which was like My First Object-Oriented
Rendering Engine. From a high level it sounded good; it used C++ and
design patterns. But it had a lot of problems.
```

Perhaps the design patterns where not the ones to be blamed, but
knowing them and using them for sure did not help =P, there is no substitute to
careful thought.

More on bloated abstractions:

```
Abstraction is powerful. What I’m really allergic to, and what I had a bad
reaction to in the ’90s, was all the CORBA, COM, DCOM, object-oriented
nonsense. Every startup of the day had some crazy thing that would take
200,000 method calls to start up and print “hello, world.” That’s a travesty;
you don’t want to be a programmer associated with that sort of thing
```

I found interesting that he had the inteligence to see that the problem is
the wrong abstractions not abstraction itself which is an essential tool for
the feeble human brain to understand large systems.

Another thing that I relate deeply is that concurrency is extremelly hard and that
concurrency with shared memory, even transactional memory, is not eh way to go and
that a better direction would be the work of Joe Armstrong on Erlang with no sharing
concurrency. It is a tradeoff, but thinking about the future it seems like the right
tradeoff.

In the end one thing that excited me is to seem someone else that is just as retarded
to interview people as I am =D. He basically just asks people about what they done on the
past and uses references from people that works with him and are smart (networking).

I just do that and seems like I'm cheating, he has the same feeling but never got
out of this level, perhaps it is OK:

```
Eich: So when I interviewed him I knew there was talent. That he came
recommended from somebody bright was good, because you know bright
people like each other and can judge each other—generally there’s not a
dysfunctional, “Hire my friend, who’s really not bright.” They want to work
with bright people. Maybe this sounds like I’m cheating, but that’s one way I
recognize talent. And I think that’s why we’re hiring superhackers. I think
we’re hiring up all the Valgrind hackers. Some of those guys can do anything;
they don’t fuck around.

Seibel: So is that something you often do in interviews: get them to talk
about their own projects?

Eich: I do. I don’t give people puzzles to solve. We have people who do
that here. To the extent that we have to do that and we’re using that to
filter applicants, I worry.

Seibel: Is that even a good first-pass filter?

Eich: I’m skeptical. Google does that in spades, and they hire a bunch of
very bright puzzle-solvers. But some of them, their street smarts are not
necessarily there, and mature judgment. So I’m skeptical of it. I think we
have to do it to some extent because you can end up getting someone who
talks well, but actually isn’t effective at programming, and so you want to
see them think on their feet, you want to see if they’ve solved a problem
before. So we give them fairly practical problems. Not esoteric puzzles or
math-y things, but more like programming problems.
```

## Joshua Bloch

Java guy with some interesting ideas:

* The first one to mention how each language is surrounded by a unique culture/community
* Very interesting insights on the complexity of adding generics to a language
* Also starting the SICP book =D


## Joe Armstrong

One thing that I identified with a lot is when he gives details on how
he has bad days and good days when developing software and that he does not
have like a steady flow of productive days. Not that you are going to sit idle
during a bad day, but it will not be as awesome as a good day. The subject comes
up when talking about pair programming :

```
Seibel: Have you ever pair programmed—sat down at a computer and
produced code with another person?

Armstrong: Yeah. With Robert, Robert Virding. We would tend to do
that when both of us were kind of struggling in the dark. We didn’t really
know what we were doing. So if you don’t know what you’re doing then I
think it can be very helpful with someone who also doesn’t know what
they’re doing. If you have one programmer who’s better than the other one,
then there’s probably benefit for the weaker programmer or the less-
experienced programmer to observe the other one. They’re going to learn
something from that. But if the gap’s too great then they won’t learn, they’ll
just sit there feeling stupid. When I have done pair programming with
programmers about the same ability as me but neither of us knew what we
were doing, then it’s been quite fun.

Then there are what I might call special problems. I wouldn’t attempt them
if I’ve got a cold or I’m not on good physical form. I know it’s going to take
three days to write and I’ll plan a day and not read email and start and it’s
gonna be four hours solid. I’ll do it at home so I know I won’t be
interrupted. I just want to do it and get into this complete concentrated
state where I can do it. I don’t think pair programming would help there. It
would be very disruptive.

Seibel: What’s an example of that kind of problem?

Armstrong: Figuring out bits of a garbage collector—it’s the imperative
coding—where you’ve got to remember to mark all those registers. Or
doing some lambda lifting in the compiler, which is pretty tough—you
relabel all the variables and then you’ve got four or five layers of abstract
data types all messing around and frames with different stuff in them and
you think, “I’ve got to really understand this, really think deeply about it.”
You want to concentrate.

I vary the tasks I do according to mood. Sometimes I’m very uninspired so I
think to myself, “Ah, who shall I go and disturb now.” Or I’ll read some
emails. Other times I feel, right now I’m going to do some hard coding
because I’m in the mood for it. You’ve got to be sort of right to do the
coding. So how’s that going to work with two people? One of them is just
not in a concentrating mode and wants to read his emails and things.
```

It is an accurate description of the problems that I have with pair programming
everyday and also how I actually work. But coming from someone as productive
as Joe Armstrong it made me feel less crappy about being like that (I always
felt like I needed to be on top performance at least 8 hours everyday, but it
has never been like that, I feel more like a roller coaster).

It is interesting that the description that he gives about how much hours of
focused work that he can do matches with the amount of hours per day
of deliberate practice that someone can (usually) do as described on the
[Talent is Overrated](https://www.amazon.com.br/Talent-Overrated-Separates-World-Class-Performers/dp/1591842948) book. It may mean nothing, but I found the coincidence interesting.

There is also a really interesting case when he and a friend that has
a very different programming philosophy worked together:

```
Seibel: You did do a kind of serial pair programming with Robert Virding,
when you passed the code back and forth rewriting it each time.

Armstrong: Yeah. One at a time. I would work on the program, typically
two or three weeks, and then I’d say, “Well, I’ve had enough, here you are,
Robert.” And he’d take it. Every time we did this, it would come back sort
of unrecognizable. He would make a large number of changes and it’d come
back to me and I’d make a large number of changes.

Seibel: And they were productive changes?

Armstrong: Oh, absolutely. I was delighted if he found better ways of
doing things. We both got on very well.He used to generalize. I remember
once I found a variable—I followed it round and round through about 45
routines and then, out it came, at the end, never even used. He just passed
this variable in and out of 45 different functions. I said, “What’s that for?
You don’t use it.” He said, “I know. Reserved for future expansion.” So I
removed that.

I would write a specific algorithm removing all things that were not
necessary for this program. Whenever I got the program, it became shorter
as it became more specific. And whenever Robert took my program it
became longer, adding generality. I believe this Unix philosophy—a program
should do what it’s supposed to do and nothing else. And Robert’s
philosophy is it should be a general program and then the program itself
should be a specific case of the general program. So he would add generality
and then specialize it.

Seibel: That seems like a pretty deep philosophical divide. Was there any
benefit to having the program go through those two extremes?

Armstrong: Oh yes. Every cycle it improved. I think it was a lot better
because of that. And probably better than either of us could have done on
our own.
```

It is kinda beautiful to imagine two forces like that balancing each other,
the different designs bouncing from one side to the other =).

He also gives good advice on the subject of opening black boxes:

```
Seibel: But do you think it’s really feasible to really open up all those black
boxes, look inside, see how they work, and decide how to tweak them to
one’s own needs?

Armstrong: Over the years I’ve kind of made a generic mistake and the
generic mistake is to not open the black box. To mentally think, this black
box is so impenetrable and so difficult that I won’t open it. I’ve opened up
one or two black boxes: I wanted to do a windowing system, a graphics
system for Erlang, and I thought, “Well, let’s run this on X Windows.” What
is X Windows? It’s a socket with a protocol on top of it. So you just open
the socket and squirt these messages down it. Why do you need libraries?
Erlang is message based. The whole idea is you send messages to things and
they do things. Well, that’s the idea in X Windows—you’ve got a window,
send it a message, it does something. If you do something in the window it
sends you a message back.

So that’s very much like Erlang. The way of
programming X Windows, however, is through callback libraries—this
happens and call this. That’s not the Erlang way of thinking. The Erlang way
of thinking is, send a message to something and do something. So, hang on,
let’s get rid of all these libraries in between—let’s talk directly to the
socket.

And guess what? It’s really easy. The X protocol’s got, I don’t know, 100
messages, 80 messages or something. Turns out you only need about 20 of
them to do anything useful. And these 20 messages you just map onto
Erlang terms and do a little bit of magic and then you can start sending
messages to windows directly and they do things. And it’s efficient as well.
It’s not very pretty because I haven’t put much effort into graphics and
artistic criteria—there’s a lot of work there to make it look beautiful. But
it’s not actually difficult.

I can’t say beginner programmers should open up all these abstractions. But
what I am saying is you should certainly consider the possibility of opening
them. Not completely reject the idea. It’s worthwhile seeing if the direct
route is quicker than the packaged route. In general I think if you buy
software, or if you use other people’s software, you have to reckon with an
extremely long time to tailor it—it doesn’t do exactly what you want, it
does something subtly different. And that difference can take a very long
time to solve.
```

And on rewriting code:

```
Seibel: Plus, ImageMagik is already written.

Armstrong: That doesn’t worry me in the slightest. I think if I was doing it
in OCaml then I would go down and do it because OCaml can do that kind
of efficiency. But Erlang can’t. So if I was an OCaml programmer: “OK, what
do I have to do? Reimplement ImageMagik? Right, off we go.”

Seibel: Just because it’s fun?

Armstrong: I like programming. Why not? You know, I’ve always been
saying that Erlang is bad for image processing—I’ve never actually tried. I
feel it would be bad but that might be false. I should try. Hmmm, interesting.
You shouldn’t tempt me.
The really good programmers spend a lot of time programming. I haven’t
seen very good programmers who don’t spend a lot of time programming. If
I don’t program for two or three days, I need to do it. And you get better at
it—you get quicker at it. The side effect of writing all this other stuff is that
when you get to doing ordinary problems, you can do them very quickly.
```

Remembering that he is a programmer that works at the industry, so it is
not a bad idea to sometimes rewrite things to learn, to have fun and even
to develop something better. I understand the economic urge to leverage
code that has already been writen, but wanting to spend you entire life
just gluying stuff together seems mediocre.


## Peter Norvig

### TDD

One of the first things that caught my attention was his experience
with people that takes TDD as a religion and a way to do design.
Even Kent Beck talks about TDD as a tool to get feedback and perhaps
write better tests that works for him, but on top of that people started
to build a whole religious tought that writing tests first drives the
design and will shape de design of the application and that is non-sense.

Nothing substitutes thinking about the design of the application and
having a good grasp of the problem that you are solving and he has a very
interesting experience on that regard:

```
It’s also important to know what you’re doing. When I wrote my Sudoku
solver, some bloggers commented on that. They said, “Look at the
contrast—here’s Norvig’s Sudoku thing and then there’s this other guy,
whose name I’ve forgotten, one of these test-driven design gurus. He starts
off and he says, “Well, I’m going to do Sudoku and I’m going to have this
class and first thing I’m going to do is write a bunch of tests.” But then he
never got anywhere. He had five different blog posts and in each one he
wrote a little bit more and wrote lots of tests but he never got anything
working because he didn’t know how to solve the problem.

I actually knew—from AI—that, well, there’s this field of constraint
propagation—I know how that works. There’s this field of recursive
search—I know how that works. And I could see, right from the start, you
put these two together, and you could solve this Sudoku thing. He didn’t
know that so he was sort of blundering in the dark even though all his code
“worked” because he had all these test cases
```

The idea is not that TDD is bad, but TDD cant solve problems, it is just
a tool. It is your mind that solves problems and nothing can substitute that:

```
Then bloggers were arguing back and forth about what this means. I don’t
think it means much of anything—I think test-driven design is great. I do
that a lot more than I used to do. But you can test all you want and if you
don’t know how to approach the problem, you’re not going to get a
solution
```

### Perfect Solutions

I identified myself with his strugles on the subject of how elegant
a solution needs to be =)

```
Seibel: How do you avoid over-generalization and building more than you
need and consequently wasting resources that way?

Norvig: It’s a battle. There are lots of battles around that. And, I’m
probably not the best person to ask because I still like having elegant
solutions rather than practical solutions. So I have to sort of fight with
myself and say, “In my day job I can’t afford to think that way.” I have to say,
“We’re out here to provide the solution that makes the most sense and if
there’s a perfect solution out there, probably we can’t afford to do it.”
We have to give up on that and say, “We’re just going to do what’s the most
important now.” And I have to instill that upon myself and on the people I
work with. There’s some saying in German about the perfect being the
enemy of the good; I forget exactly where it comes from—every practical
engineer has to learn that lesson.
```

### Code Reuse and Outsourcing

When asked about debugging he talks about when he was involved with the
postmortem of the failed mars mission on 1998 working at NASA. It is
very interesting to see such an important project with such a high
cost fail for something that makes software fail all the time everywhere,
communication. The outsourcing just exacerbated the communication problems:

```
Seibel: What is the worst bug you’ve ever had to track down?

Norvig: Well, I guess the most consequential bugs I was involved with
were not mine, but the ones I had to clean up after: the Mars program
failures in ’98. One was foot-pounds vs. newtons. And the other was, we
think, though we’re not 100 percent sure, prematurely shutting off the
engines due to a software problem.

Seibel: I read one of the reports on the Mars Climate Orbiter—that was
the one that was the foot-pounds vs. newtons problem—and you were the
only computer scientist on that panel. Were you involved in talking to the
software guys to figure out what the problem was?

Norvig: That was pretty easy, post hoc, because they knew the failure
mode. From that they were able to back it out and it didn’t take long to
figure that one out. Then there was this postmortem of why did it happen?
And I think it was a combination of things. One was outsourcing. It was a
joint effort between JPL in Pasadena and Lockheed-Martin in Colorado.
There were two people on two different teams and they just weren’t sitting
down and having lunch together. I’m convinced that if they had, they would
have solved this problem. But instead, one guy sent an email saying, you
know, “Something not quite right with these measurements, seems like
we’re off by a little bit. It’s not very much, it’s probably OK, but—”
```

And then enters code reuse for the rescue !

```
Seibel: That was all during the flight?

Norvig: Right. During the flight they had chance and chance to catch it.
They knew something was wrong and they sent this email but they did not
put it into the bug-tracking system. If they had, NASA has very good
controls for bug tracking and at later points in the flight somebody would
have had to OK it. Instead it was just an informal email that never got an
answer back, and JPL said, “Oh, I guess Lockheed-Martin must have solved
this problem.” And Lockheed says, “Oh, JPL’s not asking anymore—they
must not be concerned.”

So it was this communications problem. It was also a software reuse
problem. They have extremely good checks for the stuff that’s mission-
critical, and on the previous mission the stuff that was recorded in foot-
pounds was non–mission-critical— it was just a log that wasn’t used for
navigation. So it had been classified as non–mission-critical. In the new
mission they reused most of the stuff but they changed the navigation so
that what was formerly a log file now became an input to the navigation.
```

### Solving Bugs

He mentioned how he sometimes solve bugs without being sure where the bug
is, and it is something that I do sometimes and also feel guilty about =)

```
Seibel: On a different topic, what are your preferred debugging techniques
and tools? Print statements? Formal proofs? Symbolic debuggers?

Norvig: I think it’s a mix and it depends on where I am. Sometimes I’m
using an IDE that has good tracing capability and sometimes I’m just using
Emacs and don’t have all that. Certainly tracing and printing. And thinking.
Writing smaller test cases and watching them go, and breaking the
functionality down to see where the test case failed. And I’ve got to admit, I
often end up rewriting. Sometimes I do that without ever finding the bug. I
get to the point where I can just feel that it’s in this part here. I’m just not
very comfortable about this part. It’s a mess. It really shouldn’t be that way.
Rather than tweak it a little bit at a time, I’ll just throw away a couple
hundred lines of code, rewrite it from scratch, and often then the bug is
gone.

Sometimes I feel guilty about that. Is that a failure on my part? I didn’t
understand what the bug was. I didn’t find the bug. I just dropped a bomb on
the house and blew up all the bugs and built a new house. In some sense,
the bug eluded me. But if it becomes the right solution, maybe it’s OK.
You’ve done it faster than you would have by finding it.
```

### Literate Programming

On literate programming he gives one of the more different views that
really got me thinking:

```
You look at Knuth’s original Literate Programming, and he was really trying to
say, “What’s the best order for writing a book,” assuming that someone’s
going to read the whole book and he wants it to be in a logical order.
People don’t do that anymore. They don’t want to read a book. They want
an index so they can say, “What’s the least amount of this book that I have
to read? I just want to find the three paragraphs that I need. Show me that
and then I’ll move on.” I think that’s a real change.
```

It is not that literate programmin is useless, but it is true that in a lot
of contexts today people just want to learn what they need to accomplish X,
not the entire history about the software that hey are using.


## Common Observartions

Here are some of the things that seems to be a trend among programmers.

### Worst Bug

Almost all the worst bug experiences came from multi-threading and time sensitive
problems, which are the hardest ones to isolate and reproduce. I got one of these
that I was not even able to solve and kinda of circled around it with ugly hacks =/.

It was nice to see that other people, smart people, strugled as much with it as me
and it shows the importante of proper tooling and expressiveness from languages
to handle concurrency properly.

### Debugging

Almost all the people that is interviewed do most of their debugging with
print statements, makes me feel less alone in the world =).

Of course it may be because of the low quality of our debugging tools =/.
Almost all of them are ashamed of not using debug tools properly and also
admits that this is partially because of the quality of the tools, which is
also a feeling that I can relate to.

It is like, I'm not that idiot, gdb is really hard, even to smart people =).

### Literate Programming

Knuth's [literate programming](http://www.literateprogramming.com/)
always comes up and almost all the interviewed programmers
are interested on it. It is odd that these ideas are not more common place and
it motivated me to check it in more detail.
