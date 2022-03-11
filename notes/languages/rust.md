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

More details on [testing](https://doc.rust-lang.org/book/ch11-00-testing.html).

### Scope Blocks as Expressions

On Rust you can have scope blocks inside functions and those blocks are considered
expressions, so this would work:

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

Which is quite interesting, some initializations can be reasonably complex
and yet not warrant a function extraction. This way code keeps being local
and yet in a new/safer scope meant only for the initialization. You
get the benefit of the function extraction but without losing locality.

It does come with a tradeoffs, which answers my initial question of why does
Rust keep the semicolons:

```
Note that the x + 1 line doesn’t have a semicolon at the end, unlike most of
the lines you’ve seen so far. Expressions do not include ending semicolons.

If you add a semicolon to the end of an expression, you turn it into a
statement, and it will then not return a value. Keep this in mind as you
explore function return values and expressions next.
```

This design decision doesn't seem coupled to scope blocks, you could
have returns inside the scoped blocks to make it an expression, seems
to be more about less typing maybe (no explicit returns) since this is also
valid:

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```

This is not an immediate plus to me, but it could be some sort of prejudice
since all languages that I used so far always had explicit returns. It is
the classic tradeoff of being explicit and typing more VS implicit behavior
and less typing. Explicit returns are still allowed anyway, so you can always
use them.

### No named returns

I never liked Go's named returns, so the fact that Rust doesn't have them is
a plus to me.

### Predicates must be boolean

This is also something that feels basic to me, but so much languages to this
differently in the name of metaprogramming and typing less or something that
it is a huge plus that Rust avoided that pitfall. If you try to use a value
that is not a boolean in an **if**:

```
The error indicates that Rust expected a bool but got an integer.
Unlike languages such as Ruby and JavaScript, Rust will not automatically
try to convert non-Boolean types to a Boolean.
You must be explicit and always provide if with a Boolean as its condition.
```

Even languages that don't have type coercion, like Python, they still do it for
booleans, so you can do all sorts of crazy shit on **ifs** (essentially a
form of type coercion masked behind metaprogramming/behavior). That never
benefited me and I was used to losing a lot of time with this on Lua/Python,
so on tradeoff land I prefer being explicit on **ifs** and writing more code.

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
is more flexible than what Im used to and I have an overall bad feeling with
how messy things can get with the extra added flexibility. On the other hand,
if I'm not wrong, every time you split a modules into dirs/files then the
name of the dirs/files need to match the module path, kinda like Go/Python,
so that is a step in the right direction when compared to C++ where namespaces
can be defined anywhere, so mapping namespaces to where they are defined
can be quite challenging (then code navigation helps).

But still on the fence on this and not even sure if I got the module system
on Rust right =P. Also I need to say that seeing **super::** also does give me
a lot of Javaesque/Pythonesque inheritance chills =P. Checking bigger code bases
and how they organize things will help.

### Test Code Organization

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

Actually, given the Rust definition of unit/integration tests, what I do 90%
of the time is integration tests:

```
Unit tests are small and more focused, testing one module in isolation at a
time, and can test private interfaces. Integration tests are entirely external
to your library and use your code in the same way any other external code
would, using only the public interface and potentially exercising multiple
modules per test.
```

I like to optimize testing always towards the public facing API of libraries/services,
even when I test smaller parts of the API in isolation, I do so on the public
API. So I imagine myself doing mostly integration tests according to Rust
terminology.

For integration tests the way is to separate them on a **tests** dir
on the project root, which is what I was used to when working with Lua/Python/C/C++.

For testing binaries, Rust approach matches mine:

```
Integration Tests for Binary Crates

If our project is a binary crate that only contains a src/main.rs
file and doesn’t have a src/lib.rs file, we can’t create integration tests
in the tests directory and bring functions defined in the src/main.rs file
into scope with a use statement.

Only library crates expose functions that other crates can use;
binary crates are meant to be run on their own.

This is one of the reasons Rust projects that provide a binary have a
straightforward src/main.rs file that calls logic that lives in the src/lib.rs
file. Using that structure, integration tests can test the library crate with
use to make the important functionality available.

If the important functionality works, the small amount of code in the
src/main.rs file will work as well, and that small amount of code
doesn’t need to be tested.
```

Overall the testing approach relies a lot on conventions, which is a good
way to keep projects consistent, like Go with `name_test.go`. One interesting
limitation is that integration tests, located at the **tests** dir can't have
tests defined in subdirs, so you can do all your integrated testing using just
multiple files on the tests dir:

```
Files in subdirectories of the tests directory don’t get compiled as separate
crates or have sections in the test output.
```
This make it easy to create helper modules for the tests, but if you have
a lot of integration tests I'm not sure that only using multiple files
in the same directory would scale well, maybe there is some way to
circumvent this (maybe using the test annotation on the subdirs ?).
