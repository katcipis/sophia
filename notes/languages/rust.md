# Rust

Some of my personal experiences with Rust, what I liked, what I didn't, lessons
learned, etc.

## The Good

Of course "Good" is always subjective to taste, not in the sense that we are
talking about art, but that even on engineering some architectural
constraints/properties are more appealing to some people because they value them
more. There is no architectural property that is intrinsically better than
other (unless it is just plain dumb architecture) but since these are
my notes they are biased on properties that I like.

### Private by default

This is a good example of trade-off that appeals to me but it is not
universally good. Everything in Rust is private by default, I like that
because I have a strong bias towards isolation. It helps me feel safer
as I navigate code when I can make it as isolated as possible and
limit the surface of APIs. I prefer extra work to make things public
than accidentally leaking something.

Some more details on pub:

* https://doc.rust-lang.org/book/ch07-03-paths-for-referring-to-an-item-in-the-module-tree.html#exposing-paths-with-the-pub-keyword

### Testing

I'm still assessing but so far the testing support seems simple enough
and the stdlib seems to provide a good balance towards providing something
useful but now way too much, it doesn't feel very frameworky.

It has basic assertion macros, that use the language default comparison
operators/traits for equality and provides basic support for testing
code that creates/errors panics, including partial matching on panic
messages, which can be useful.

More details on testing:

* https://doc.rust-lang.org/book/ch11-00-testing.html

## The Fence

Things that I'm still on the fence about.

### Modules

I have mixed feelings with **mod** just as I always had with C++ namespaces.
In general I like namespaces, specially as a way to keep private
symbols small by adding then on their own namespace. Languages like
Go/Python force you towards creating separate files or directories to
create namespaces (Python is dir + files, Go only dirs). Languages
like C++ and Rust decouple both things, creating namespaces and isolating
symbos is unrelated to source files/directories AFAIK.

That does provide a great deal of flexibility and it is very easy to isolate
a few private functions together in the same namespace without new files/dirs,
but can create a situation where finding symbols could be hard since files/dirs
won't help you much. Code navigation features on text editors/IDEs mitigate this,
so maybe it is not a huge issue, but it does remind of Java in a bad way, like
you CAN'T work with the language without an IDE, rich IDEs should be optional not
a requirement.

But still on the fence on this and not even sure if I got the module system
on Rust right =P.
