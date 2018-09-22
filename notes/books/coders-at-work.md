# Coders at Work

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