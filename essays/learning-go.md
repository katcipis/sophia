# The Go learning experience

We started to work at Neoway with Golang for almost a year right now, and on this post
I will try to pass on some of the experience of learning Go.

To make things easier I will use a very clear example, one problem, and some context
on what was happening.

By no means this is a representation of the experience of everyone at Neoway, there is a lot
of teams working with Go. This is a very narrow and personal view on how was to me the process
of learning and developing Go code.


## Context: The Datapirates Team


Our team is responsible for capturing data on the web. So we basically develop
scrapers and all services required for the scrapers to work properly
(like captcha breaking, proxy providing, etc).

On this essay I will focus only on the scrapers, not the rest of the services we
maintain, neither the rest of the data pipeline that we have at Neoway.


## Context: The scrapers


To scrap data from the web we decided to use [Scrapy](http://scrapy.org/), which uses
Twisted to handle the heavy I/O based workload without requiring a ridiculously amount of
threads to enable high concurrency. I wont get in much detail on these guys right now,
but if you are interested they are fairly well documented.


## Why Go ?


As you can see, our scrapers are developed in Python, and Python has a lot of useful tools
not only to crawling but also parsing (we parse all kinds of things that the web throws at us,
like PDFs and SWF files). Well, as I said before, we have to maintain other services that are
required for the scrapers to work properly, and since we are on a architectural migration
(more on that on future essays, I think) there was a lot of new services to be developed.

So we are not thinking on developing scrapers on Go (although that sounds like fun :-)), but
was aiming at these other services (spoiler alert: in the end almost all services on our 
data pipeline are written in Go).

Having made that clear, why Go ? Here goes a list of things I thought to pretty cool about Go:


### Great performance with good abstractions

This one may seem like some kind of premature optimization, all that talk about you should not care
with performance, only solving your problem fast, etc (oh the joys of the tradeoffs on delivery speed
and software speed) does not make that much sense when you **literally** pay for your computing power
hourly.

If you can be 10x times faster, there is a fairly good chance that you will spend 10x less money.
This has always been truth, I think the cloud model only makes it
even more explicit, specially as you start working with elastic infrastructure
(but even on the pre-cloud era it was smart to think about that, instead of just making stuff to work,
take a look on the great [Release It](https://pragprog.com/book/mnee/release-it) for more on that).

Of course this has to be balanced with good abstractions, or we would be developing everything on C
until today (it is fun, but not very fast, specially if you intend to your software to work properly :-)).
Go seems to match these two things very nicely, I heard a lot of cases from people migrating from Python
and Ruby to Go and getting huge improvements on performance, and still being able to develop software
fast, thanks to well chosen abstractions.


### Stupidly simple deployment

As someone with C background, but that also have experience with interpreted languages like
Python, Javascript (NodeJS/Backend stuff) and Lua, I value a lot the model of just copying a
binary and running it.

I worked with these interpreted languages on the more harsh and odd places, like old kernels
with proprietary crappy packaging systems where the norm. It was hell and the stuff that I had
to make just to get shit working takes my sleep at night (not literally of course, I love drama :-)).

To be honest, even in C it was a hell because of dynamic libraries, but even that Go got right, it was
completely static, and it had a compile mode that even the libc was embedded on the binary. It was
the glory of the simplicity on deploying something.

The addition of libraries (static and dynamic) is fairly new on Go (it even got dynamic loading, yay the
joys of dynamic loading), when we started with it they did not even existed (and for me it was
perfect that way).


### Batteries included

Pretty much the basic you will need to develop services on the web and test code.


### Static Typing with Interfaces

I just got fascinated on how Interfaces are implemented on Go. The idea of not requiring
an object to directly know and import/extend an interface to implement is the glory
of dependency inversion.

This is not new, since it is what dynamic languages are doing
for a long time, which is duck typing. Go has duck typing with static validation of
your ducks, if they are quacking properly, etc. 

You can have good tests to aid you on the dynamic language, but I never heard anyone
complaining because a static analysis helped them identify a problem really fast with
no development effort required.


### Simplicity

This is the greatest reason for me to like something, simplicity. Simplicity on the terms of:

* Only one way to do stuff
* Being explicit
* Being consistent
* As few primitives as possible
* As few abstractions as possible

Of course, enabling you to write solutions to difficult problems without imposing overheads
on you. C for example has almost no abstractions, if you need to express abstractions it is not
trivial to do that.

The idea is not to have no abstractions, but only the minimum required to solve problems elegantly.
A good example is Lua, where almost everything is represented as tables, objects, modules, etc.
Importing modules is just a function returning a table mapping names to functions.

There is abstractions on this, but as few as possible, and you can still do everything (and more)
that you used to do with languages that have more abstractions (like Python and Javascript, for example).

Even the global namespace in Lua is just a table, so doing fun meta-programming stuff is explicit and
trivial (although not recommended :-) on most cases).

The idea here is not to focus on Lua, but it is another language where simplicity is beautifully expressed.

So the idea of having less abstractions is having the necessary to solve complex problems, not the absence
of abstractions. In Go, good examples of simplicity are:

* Object model using Interfaces, instead of inheritance
* The concurrency model
* [Errors are just values](https://blog.golang.org/errors-are-values), instead of exceptions
* High order functions, instead of decorators
* The defer statement


### The who and why

https://commandcenter.blogspot.com.br/2012/06/less-is-exponentially-more.html


### And of course, concurrency :-)


## Context: Use Go where ?
