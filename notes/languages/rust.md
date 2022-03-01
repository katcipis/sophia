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

#### Test Code Organization

On how to organize tests, from [The Rust Programming Language](https://doc.rust-lang.org/book/ch11-03-test-organization.html):

```
You’ll put unit tests in the src directory in each file with the code that
they’re testing. The convention is to create a module named tests in each file
to contain the test functions and to annotate the module with cfg(test)
```

I find the decision interesting, specially for unit tests. Keeping them as
close as possible makes things easier, specially since they change together,
and Rust module system and annotations allows this to be done safely, in
a way that the code under test is unaffected by the tests residing on the
same file (no extra symbols exported or anything).

It does seem that keeping tests on the same file as a submodule does
make it remarkably easy to import private symbols on the tests and
then test private stuff, I'm considerably against testing private
stuff since most of the time it is a bad idea IMHO, but not always,
so it makes sense to provide a way to do so, but in this case it ends
up remarkably easy to do so. More info
[here](https://doc.rust-lang.org/book/ch11-03-test-organization.html#testing-private-functions).

## The Fence

Things that I'm still on the fence about.

### Modules

I have mixed feelings with **mod** just as I always had with C++ namespaces.
In general I like namespaces, specially as a way to keep private
symbols small by adding then on their own namespace. Languages like
Go/Python force you towards creating separate files or directories to
create namespaces (Python is dir + files, Go only dirs). Languages
like C++ and Rust decouple both things, creating namespaces and isolating
symbols is unrelated to source files/directories AFAIK.

That does provide a great deal of flexibility and it is very easy to isolate
a few private functions together in the same namespace without new files/dirs,
but can create a situation where finding symbols could be hard since files/dirs
won't help you much. Code navigation features on text editors/IDEs mitigate this,
so maybe it is not a huge issue (and you can always properly organize code),
but it does remind of Java in a bad way, like you CAN'T work with the language
without an IDE, rich IDEs should be optional not a requirement.

There seems to be a way to map module names directly to the filesystem/filenames:

* https://doc.rust-lang.org/book/ch07-05-separating-modules-into-different-files.html 

So it would make things more predictable, but it doesn't seem to be enforced so
I'm not sure how far code in general uses it to organize module code in a project.
Overall the whole module system, how to define things and how to "import"/"use" them
is more flexible than what Im used too and I have an overall bad feeling with
how messy things can get with the extra added flexibility.

But still on the fence on this and not even sure if I got the module system
on Rust right =P. Also I need to say that seeing **super::** also does give me
a lot of Javaesque/Pythonesque inheritance chills =P. Checking bigger code bases
and how they organize things will help.
