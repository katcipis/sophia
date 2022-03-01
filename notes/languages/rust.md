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
