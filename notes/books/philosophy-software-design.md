# A Philosophy of Software Design

The book [A Philosophy of Software Design](https://www.amazon.com/Philosophy-Software-Design-John-Ousterhout/dp/1732102201)
starts really well, admitting that in the field of
software engineering/computer science very little is discussed
about software design, the core issues that it addresses.

There is a lot of talk about magic solutions, patterns, styles,
that are usually presented (or used) as silver bullets and
don't go deep in what is the reasoning behind modularization, it is
just a recipe for you to follow. Recipes and rules are great when
you are starting, continuing with them forever is a terrible idea,
and the industry seems stuck on this first level, processes and
silver bullet methods/patterns.

The author wants to propose that we start discussing software design
again, at its core, which in his opinion is problem decomposition. There
is no single class about problem decomposition on computer science
courses (or bootcamps, obviously), and that is the core of true
mindful modularization in software. What is the objective of
modularizing ? What would be a good criteria for it ?

One of the last great papers about this, which is also mentioned
by the book, is Parnas
[On the Criteria To Be Used in Decomposing Systems into Modules](https://web.archive.org/web/20120223013018/https://www.cs.umd.edu/class/spring2003/cmsc838p/Design/criteria.pdf). I read it a few years ago and I admit it is truly
one of the best materials on how to modularize software I ever read.
It is a honest comparison of different strategies, where the judge
of the quality of the design is change, the thing that always cripple
software projects, change. It has no cute religious crap like "clean",
stuff that will make you feel good about yourself, it is true engineering,
just an exposition of tradeoffs, and we haven't progressed much on this
regard in the author's opinion, and I agree.

The idea of the book is to discuss software design at a high level, a conceptual
level, the level of ideas, which is great because it is like learning how to
think instead of learning something that you are going to just repeat
forever in different contexts (design patterns, MVC, etc). I think that
is why he talks about philosophy, it is about thinking on how you think,
finding new ways of thinking, it is impossible to really assess
software design without meta cognition, thinking about form is thinking
about how humans understand form, be it physical of abstract.

This reminds me of a quote from the article
[The Computer Scientist as Toolsmith](https://cs.unc.edu/xcms/wpfiles/toolsmith/The_Computer_Scientist_as_Toolsmith.pdf),
it defines us (computer scientists, programmers, etc) as "engineers of abstract objects".
We build things but they exist on the real of abstraction, so is the
form of the built artifacts, this makes it specially hard
to design software, we are not that great with abstractions
in general, just better than common apes.

One more digression before going on in the books ideas. Specially
today people may feel that talking about philosophy is useless,
you need to just learn how to build stuff and actually do something,
like philosophy is just some useless mind exercise. I agree with
Paul Graham vision that philosophy should not be vague, philosophy should
be extremely practical, it is thinking about how you think, why you
think that way, in a way that can change you, improve you, and others
around you. So in a sense it is one of the hardest and more
important exercises you can do, but it is extremely hard indeed.

## Creation

I resonate with his view that programming is one of the most
creative activity humans have been evolved since ever. The computer
is a very expressive medium and there is very little physical limits
to building software, if you have creativity and you are able to
organize your thoughts, you can build software.

It is like the painter with a blank canvas, but the canvas is gigantic
and multi dimensional, so the space to explore is vast. This reminds me
of a quote from [Structure And Interpretation Of Computer Programs](https://mitpress.mit.edu/sites/default/files/sicp/full-text/book/book-Z-H-3.html):

```
I think that it's extraordinarily important that we in computer science
keep fun in computing. When it started out, it was an awful lot of fun.

Of course, the paying customers got shafted every now and then, and after
a while we began to take their complaints seriously. We began to feel as
if we really were responsible for the successful, error-free perfect
use of these machines.

I don't think we are. I think we're responsible for stretching them,
setting them off in new directions, and keeping fun in the house.
I hope the field of computer science never loses its sense of fun.

Above all, I hope we don't become missionaries. Don't feel as if you're
Bible salesmen. The world has too many of those already.
What you know about computing other people will learn.
Don't feel as if the key to successful computing is only in your hands.
What's in your hands, I think and hope, is intelligence: the ability to
see the machine as more than when you were first led up to it,
that you can make it more.
```

We have an amazing medium in front of us, using it to find "right answers"
that makes building products fast is mediocre and depressing, at least for me,
it is the opposite of innovation (although the industry only talks about inovation,
it does not feel very innovative almost all the time). And ironically,
using these canned ideas sold by "Bible Salesman" makes build products
slower almost all the time, if the product has any real challenge on it,
so exploring the design space is not just about having fun, it is about
learning what is truly simple and minimal.
