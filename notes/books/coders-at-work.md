# Coders at Work

These notes are based on my reading of the book
[Coders at Work](http://www.codersatwork.com/).

The first impression that I get from the preface of the book is how
young our field really is. There is an entire generation of people alive
that was born on a world where there was no computers and we are essentially
very near of the first generation of programmers.

For a field that has impact in human life as computing have this is very thrilling,
being part of the beginning of something so awesome. It reminds me of
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
thing to the other.

I identified myself with him a lot. He really appreciates good stuff like Scheme
and Smalltalk, and he is so against traditional OO and Java and design patterns
that he made a lot of effort to not allow things like classes in Javascript
(he lost in the end).

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

He was the only interviewee that talked about how
each programming language is surrounded by a unique culture and community
and that is part of the choice for a language, if it fits you.

That is something that I perceived personally when I was learning Lua,
I could compare with Python and the  community is severely different from
each other, and even attract different kind of people.

The Lua guys are much more like C programmers, where
testing is not seem like a religion and the solution to a lot of problems
is to just roll your own library instead of using a heavily
supported mainstream library, also they dislike traditional OO
with inheritance. The Python community is pretty much the opposite.

Besides that he also gives a very interesting take on how hard is to
add support to parametric polymorphism (generics):

```
In particular, the Java 5 changes
added far more complexity than we ever intended. I had no understanding
of just how much complexity generics and, in particular, wildcards were
going to add to the language. I have to give credit where credit is due—
Graham Hamilton did understand this at the time and I didn’t.
```

It is kinda sad that in face of a change that ended up being much
more complex than intended they didn't just rollback the language
to a previous state, to try something more simple.

A little more on the naiveness of adding generics:

```
Seibel: So what was the impetus for adding generics?

Bloch: As is always the case for ideas that prove less wonderful than they
seemed, it was believing our own press sheets. My mental model was, “Hey,
collections are almost all homogeneous—a list of strings, a map from string
to integer, or whatever. Yet by default they are heterogeneous: they’re all
collections of objects and you have to cast on the way out and that’s
nonsense.”

Wouldn’t it be much better if I could tell the system that this is a
map from strings to integers and it would do the casting for me and it
would catch it at compile time when I tried to do something wrong? It could
catch more errors—it would have higher-level-type information and that
sounds like a good thing.

I thought of generics in the same way I thought about many of the other
language features we added in Java 5—we were simply getting the language
to do for us what we had to do manually before. In some cases I was dead
on: the for-each loop is just great.

All it does is hide the complexity of the
iterators or the index variables from you. The code is shorter and the
conceptual surface area is no larger. In a sense, it’s even smaller because
we’ve created this false polymorphism between arrays and other collections
so you can iterate over an ArrayList or an array and not know or care
which you’re iterating over.

The main reason this thinking didn’t apply to generics is that they represent
a major addition to an already complex type system. Type systems are
delicate, and modifying them can have far-reaching and unpredictable effects
throughout the language.

I think the lesson here is, when you are evolving a mature language you have
to be even more conscious than ever of the power-versus-complexity
trade-off. And the thing is, the complexity is at least quadratic in the number
of features in a language. When you add a feature to an old language you’re
often adding a hell of a lot of complexity. When a language is already at or
approaching programmers’ ability to understand it, you simply can’t add any
more complexity to it without breaking it.
```

I find specially interesting the idea of thinking that the complexity
of adding a new feature to a language (or any system) is quadratic.
This is a over simplification but it is a useful one, it reminds us
that when you add a feature to something you have the complexity of
the feature multiplied by its combination with all the previous features.
On systems that are more restricted you can isolate features better, but
languages are usually so generic that you can't avoid different features
being mixed, and defining the behavior of the different permutations of
features being used together. There can be some isolation, but it seems
much harder on a language because of the flexibility usually given on
what can be expressed.

And he is yet another interviewee to talk about how C++ is so complex
that it always need to be subset ed before being used:

```
And if you do add complexity to it, will the language simply disappear? No, it
won’t. I think C++ was pushed well beyond its complexity threshold and yet
there are a lot of people programming it. But what you do is you force
people to subset it. So almost every shop that I know of that uses C++ says,
“Yes, we’re using C++ but we’re not doing multiple-implementation
inheritance and we’re not using operator overloading.” There are just a
bunch of features that you’re not going to use because the complexity of
the resulting code is too high. And I don’t think it’s good when you have to
start doing that. You lose this programmer portability where everyone can
read everyone else’s code, which I think is such a good thing.
```

Which was the spark that started the Go language, I sure hope it keeps
that (as I write this there are ongoing discussions for adding
parametric polymorphism on the language, for example). To finish the
subject of language design:

```
But I think many of us have learned from our experience
with generics. You shouldn’t add something to a language until you really
understand what it’s going to do the conceptual surface area—until you can
make a convincing argument that working programmers will be able to use
the new feature effectively, and that it will make their lives better.
```

It is sad to see so much languages ignoring this advice and just getting
more and more features that do not even compose well.

Another cool thing is his suggestion for reading:

```
Another is Elements of Style, which isn’t even a programming book. You
should read it for two reasons: The first is that a large part of every
software engineer’s job is writing prose. If you can’t write precise, coherent,
readable specs, nobody is going to be able to use your stuff. So anything that
improves your prose style is good. The second reason is that most of the
ideas in that book are also applicable to programs.
```

Which I could not agree more and has been added to my endless list
of books to read.

I also liked how the idea of doing something that works fast is not
related to writing stuff without thinking about it, or even without doing
any upfront design:

```
Nobody likes sloppy software. People who say, “Write the simplest thing
that could possibly work and refactor mercilessly” aren’t saying, “Write
sloppy code,” and they aren’t saying, “Don’t do upfront design work.” I’ve
talked to Martin Fowler about this. He’s a huge believer in thinking about
what you’re going to do so your system has a reasonable shape and a
reasonable structure. What he’s saying is, “Don’t write 247-page specs
before writing a line of code,” and I agree.
```

To finish I found interesting that he was also starting to read the
Structure and Interpretation of Computer Programs book late in his life,
like I'm doing right now.


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

I identified myself with his struggles on the subject of how elegant
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


## Guy Steele

The thing that caught more my attention on his interview was when he
was asked about tools that helps everyone write programs. For start
it is hard to define what would be to "write a program", some niches
and domain specific languages can make it pretty easy to write some
types of programs, but would it qualify a person as a programmer ?
Even defining this is hard.

But the point that he makes is that it is hard to make generic programming
something accessible to everyone because programming is a very unusual
activity, so it is not spread in the skill set of our species as evenly
as other skills. One of the sources of the unusualness on his opnion is the
error handling, edge cases and correctness. Building a mindset for these
things is extremelly hard and people that are good at it don't come as
often. To be honest that makes sense, it is like expecting that everyone
will be a great mathematician. 

Even though mathematics developed a lot of new tools that makes things
easier, like calculus, still it is very hard to most humans to grasp it.
It got easier, but it is still hard, specially to do it properly and in
a generic way (not just applied to a very specific problem).

Attention to detail, edge cases, logical thinking and abstractions
don't come easily on human beings.

In more detail:

```
Seibel: Yet lots of people have tried to come up with languages or
programming systems that will allow “nonprogrammers” to program. I take
it you think that might be a doomed enterprise—the problem about
programming is not that we haven’t found the right syntax for it but that
people have to learn this unnatural act.

Steele: Yeah. And I think that the other problem is that people like to
focus on the main thing they have in mind and not worry about the edge
cases or the screw cases or things that are unlikely to happen. And yet it is
precisely in those cases where people are most likely to disagree what the
right thing to do is.

Sometimes I’ll quiz a student, “What should happen in this case?” “Well,
obviously it should do this.” And immediately someone else will jump in and
say, “No, no, it should do that.” And those are exactly the things that you
need to nail down in a programming specification of some process.

I think it’s not an accident that we often use the imagery of magic to
describe programming. We speak of computing wizards and we think of
things happening by magic or automagically. And I think that’s because being
able to get a machine to do what you want is the closest thing we’ve got in
technology to adolescent wish-fulfillment.

And if you look at the fairy tales, people want to be able to just think in
their minds what they want, wave their hands, and it happens. And of
course the fairy tales are full of cautionary tales where you forgot to cover
the edge case and then something bad happens.
```

The catch is that people usually don't want to think about the edge and error
cases but we can't automate/abstract them (I'm looking at you Istio =P) because
we can't even come to a consensus on how to handle them, it may even be
a domain specific problem to define what to do (can't be generalized).

So abstracting away from the programmer the handling of edge cases and
errors seems like a fundamentally bad idea. If it was possible it would
allow more people to program, but it seems inherent to the activity
of programming, at least if you want something that works =P (a lot of
mobile apps can survive not being resilient).

And here he talks about correctness, attention to detail and even thinking
recursively:

```
Programming is a highly unnatural activity, I’m convinced, and it must be
carefully learned. People are used to their listeners filling in the gaps. I
suppose we lean on compilers to do that in a little way—you say, “I need a
variable named ‘foo’,” you don’t worry about exactly what register and so
forth.

But I think that most people are not used to being very precise and
rigorous in their communications. But when we are describing processes to
be carried out, little details do matter because a change in a small detail can
affect the gross outcome of the process.

I think people are used to using recursion in a limited way—I think Noam
Chomsky demonstrated that. But in practice people rarely go even three
deep—and when they do it’s usually in a tail-recursive way. The discipline of
understanding recursion is actually a very difficult learned art. And yet that
is actually one of our most powerful programming tools, once you’ve
learned the discipline and wrapped your head around it.

So I really think you can’t afford to take your eye off the correctness ball.
```

And to finish, some old on language wars:

```
Seibel: One way to resolve that is the way Lisp does—make everything
uniformly semiconcise. Where the uniformity has the advantage of allowing
users of the language to easily add their own equally uniform, semiconcise,
first-class syntactic extensions. Yet a lot of folks resist the s-expression
syntax. The smug Lisp weenie view of the world is, “Some people just don’t
get it; if they did they would see the brilliance of the solution.” Are you a
smug enough Lisp weenie to think that if people really understood Lisp they
would not be put off by the parentheses?

Steele: No. I don’t think I’ve got the standing to be smug. If anything,
because I have learned so many languages I think I understand better than a
lot of people the fact that different languages can offer different things.
And there are good reasons to make choices among them rather than to hold up
one language and say, “This is the winner.”
```

## Dan Ingalls

Dan worked directly with Alan Kay at Xerox PARC as part of the development
team working on SmallTalk. Some cool projects mentionted are:

* [Squeak](https://en.wikipedia.org/wiki/Squeak)
* [Lively Kernel](https://www.lively-kernel.org/)

He is very enthusiastic about kernels, what they are composed off, what
is part of a kernel and what is not and essentially what is a good
kernel. He does not goes in great detail on what a good kernel is but
that can be seen on his work like the
[Lively Kernel](https://www.lively-kernel.org/).


### Building things that no one wants

One interesting history from him is the start of his career where he was
extremelly excited about profilers and built one of the first ones
for Fortran, just to find out that Fortran developers for several
reasons where not that interested on profilers, so in the end
he was not able to sell the profilers. Later he did one for Cobol I think,
and that one sold well. It caught my attention because it is a repeating
pattern of hackers developing what they think it is fun and finding
out that it is not sellable. Nothing wrong with doing fun stuff, just
don't expect people to buy it just because you think it is fun or useful.


### Creativity VS Reuse/Effiency/Standards

He gets on this subject on a very interesting way, it is when they are
talking about the creation of SmallTalk and how the only language
that had similar ideas was Lisp and that he was happy that he was not
"into" Lisp because it would have influenced him in a way that some
ideas that happened in SmallTalk would not have happened with Lisp
influence. It is like the price to pay for reuse of ideas, you start
with some interesting ideas but it is hard to do that without closing
doors to others. Thinking about hacking as compared to painting it would
be like starting with a blank canvas or with some painting already
done for you. The blank canvas is scary and you have more work, but more
possibilities. The canvas with already some work on can be inspiring and
allow you to go faster, but it will also steer you in a specific direction.

The context here is talking about imediate gratification and feedback
(REPL interpreters for example):

```
Seibel: So what was that first place that you had that immediate
gratification?

Ingalls: There were a couple of experiences. I had a chance to work with a
semi-interactive PL/I. And a friend of mine was working at an IBM where
they had an interactive APL environment. I can’t remember which of those
was first. I really remember the APL one. That affected me in a number of
ways because of both the immediacy of interaction—seeing your results
come back—and the expression evaluation, which is really different from
Fortran’s statement-oriented programming.
You still see that now, it’s amazing—the whole C/C++/Java tradition,
although it goes in an object-oriented direction, is still statement-oriented.
But if it’s convenient to make expressions, it really makes the experience
different. To me it brings the mathematics to life more. Anyway, it was one
of those two. When I got to Xerox there wasn’t much interactive except
the Lisp guys’ stuff. I happened not to be into Lisp. Things would have been
different if I were, I expect.

Seibel: How so?

Ingalls: I think I would have gone completely in that direction. And by not
having gone in that direction, it left me wanting to do that kind of thing in a
different way. I think what I worked on with Alan had that same kind of
nice, lively, expression feel, but it included the notion of objects and
messages more naturally.
I think if I had been as comfortable in a system like Lisp, I never would have
bothered. I would have tried working around it to get objects, but starting
with the notions of objects from the get-go and then making that nice and
interactive and convenient was, I think, a contribution.
```

Lisp was not a standard but you can extrapolate to the notion of
being ignorante about stuff and just building something from scratch
can be benefitial in some cases (but it will generate waste):

```
Seibel: So that’s a nice example of the advantage of some ignorance; it
leaves some room for creativity. But sometimes it feels like ignorance is
endemic in this industry—that people are unaware of things and wheels are
constantly being reinvented with pointy corners.

Ingalls: That’s true.

Seibel: Should we be trying to fix that? Is it just a price that we have to pay
so that people can have room for creativity? Or would we be better off if
people were a little more aware of what else is going on?

Ingalls: I’m an embrace-diversity guy. The example we just went through, I
think, says, “No, let people go whatever way they want.” There will be
waste from ignorance, but natural selection will take care of straightening
things out. And you will get these occasional sports that take you into the
future.

I can think of lots of areas where trying to standardize, trying to all go in
one direction, has suppressed creativity. I work at a company that’s
supported by Java, so I’m not going to go very far with this, but it is the case
that when Java came out—the easiest way I had of measuring it was looking
at what went on at OOPSLA—when Java came out not only did work slow down
and even maybe come to a halt on other object-oriented
programming languages but even in dynamic programming in general. I think
that was a loss.

But it wasn’t permanent. People finally realized “Oh, wait a minute, Java has
all these great strengths and we’re using it for this and that but we were on
to other good things with dynamic programming languages and it’s time to
get back to that.” But that’s an example that’s easy for me to see because
I’m involved in it.

In a much bigger sense I hate to see computer-science departments that feel
their role is to prepare people to work in an industry and the industry is
going that way and therefore we have to teach our students that way. It’s
exactly the wrong thing to do. What you should be doing with your
students is teaching them to think generally—think outside the box and plot
the other courses we should be pursuing.

It’s not a simple problem because there is great value to having that whole
standard out there. Having thousands of programmers who have worked on
thousands of routines and they’re very solid—you can get your work done
that way.

It’s a little bit the difference between computer science in the service of
production and computer science in the service of moving the intellectual
content forward. I’ve been having fun with the Lively Kernel, which is, in a
way, trivial. There’s nothing new about it—I’m using all stuff that was there
before. It’s built on JavaScript and the graphics you can get in a browser. But
it’s been really fun because it’s another kernel like Squeak. Because
JavaScript is all there in the browser and the graphics is all there in the
browser, the work on the kernel is very tiny. 

It’s just, how do you bring the graphics to life and how do you build up a
little computing environment.
Whenever you do something like that, small enough, then anybody can
understand it. If you take a few things out of the equation, like the language
and the graphics, then the question is, what is the kernel? And I think that’s
an interesting question.
I’m hoping that this—not particularly my stuff—but this kind of investigation
might reinspire computer science to do some studies about how do you
make a kernel, what other kernels could we build that are even simpler,
even more uniform.
It’s like what math did. Math found by symbolizing things that you could
simplify a lot of stuff. Then, because of that, you could start to think about
bigger constructs. That’s my hope.
```

It is one of the best expressed ideas on the balance between creativity
and moving things forward and also the value of standards. It reminds me
of Paul Graham function model of creativity, where you have something like
a senoid signal where you sometimes lose a lot and sometimes gain a lot,
if you damp the signal you start to lose less...but you gain less too
(no free lunch).


## Common Observations

Here are some of the things that seems to be a trend among programmers.
There aren't much of them and that is one of the most interesting aspects
of this book, there is very little consense on how software should be
developed or even on how to learn it. Some of the interviewee's studied
formal CS and read books, others did not read books at all (like Dan Ingalls).
But some patterns emerged =) (although nothing is ubiquotous).


### Worst Bug

Almost all the worst bug experiences came from multi-threading and time sensitive
problems, which are the hardest ones to isolate and reproduce. I got one of these
that I was not even able to solve and kinda of circled around it with ugly hacks =/.

It was nice to see that other people, smart people, strugled as much with it as me
and it shows the importante of proper tooling and expressiveness from languages
to handle concurrency properly.

The NASA mars mission that failed because of communication problems is also
very interesting, specially because communication problems are the source of
99% of my problems with software development.


### Debugging

Almost all the people that are interviewed do most of their debugging with
print statements, makes me feel less alone in the world =).

Of course it may be because of the low quality of our debugging tools =/
(as pointed out by Dan Ingalls).

Almost all of them are ashamed of not using debug tools properly and also
admits that this is partially because of the quality of the tools, which is
also a feeling that I can relate to.

It is like, I'm not that idiot, gdb is really hard, even to smart people =).


### Literate Programming

Knuth's [literate programming](http://www.literateprogramming.com/)
always comes up and almost all the interviewed programmers
are interested on it. It is odd that these ideas are not more common place and
it motivated me to check it in more detail.


### C++

It is almost unanimous that C++ is so complex that it
should not be used or at least you need to find the correct
subset of the language for you before using it =P.

Why it is used so heavily ? I suppose people, specially engineers,
like to feel smart, and mastering C++ makes you feel like a god.
No one wants to stop being a god and using something that any
peasant can use =P.
