# jsonnet

These are some personal notes on [jsonnet](https://jsonnet.org/).
It is remarkably hard to find something good to deal with
duplication on configuration/description of deployments.
That is why I ended up taking a look into jsonnet, to my
surprise, overall, it is much better than I expected.

I usually dont expect much because almost everything that I tried
on this realm (like template engines, kustomize, etc) disappointed
severely.

Having said that, the notes are mostly based on reading the
[language reference](https://jsonnet.org/ref/language.html), I don't
have much personal experience with it yet.

I really liked the fact that the language is functional/declarative
with no side effects/IO. These are restrictions that usually you can't impose
in a general purpose language (you always need some IO), but for a language
aimed specifically at configuration you can afford to do it
and just used explicitly injected parameters on build time.

It has a computing model that reminds me of substitution model of lisp:

```
Functions in Jsonnet are referentially transparent,
meaning that any function call can be replaced with its definition,
without changing the meaning of the program.

Therefore, in some sense, functions in Jsonnet are hygienic macros.
```

And everything is an expression:

```
Jsonnet programs are composed entirely out of expressions.
There are no statements or special top-level declarations present in
most other languages.

For instance, imports, conditionals, functions, objects, and
locals are all expressions.
```

It is a very simple computational model that it is usually very
easy to reason with when debugging.

I don't have an strong opinion on it, but the language is lazily evaluated,
which does enable some interesting idioms like:

```
The arguments are not evaluated when a function is called.
They are passed lazily and evaluated only when used.

This allows expressing things which in other languages
require built-in functionality or macros, such as short-circuit
boolean operations:

local and3(a, b, c) = a && b && c;
and3(true, false, error "this one is never evaluated")
```

## Types

You don't have types explicitly defined on variable declarations,
so the language feels dynamically typed, but since the language
is also purely functional you don't get traditional dynamic typing.

A variable underlying value can't change, so once a variable x is 5,
it will always be 5 on a scope, and will always have the type Number,
so in a sense the types are not actually dynamic, because the variables
themselves are not dynamic, you just never declare variable types
explicitly.

There is no type coercion, values of different types are never equal,
that is one of the very few things I never liked in Lua for example,
so for me this is a big win.

Also only booleans can be used on **if** expressions, which is also
a win for me. Even though sometimes you can have shorter ifs, I never
liked being able to do things like this (Python):

```
a = []
if a:
    print("yay")
```

For me that introduces a lot of overhead when trying to understand program
behavior, because for each type you need to understand how it behaves
when it is evaluated as a boolean expression. It gets worse when you can
meta program the behavior. You can do a lot of cool things that later
will be really hard to reason with (in general, in my experience, at least).

So another big win on if expressions.

### Strings

String are uniform arrays of unicode code points. That is considerably more
memory hungry but it is a very sane/uniform way to implement strings with
great support to i18n. Go for example is more efficient, but more confusing,
since sometimes you get "array of bytes" behavior and sometimes you get
"array of runes (unicode)" behavior (more [here](https://github.com/katcipis/sophia/blob/master/notes/go.md#strings)).

Of course this is a tradeoff, but for a language more domain specific like
jsonnet I agree with the decision of being uniform/simple to handle and using
more memory.

### Numbers

Numbers are always [IEE 754](https://en.wikipedia.org/wiki/IEEE_754) float.
They are not explicit on why, but I would guess it is inherited from JSON
itself, where you only have a Number type always represented as a float.

One win on the design of numbers is that at least any operation producinc a NaN
or an Inf creates an explicit error, instead of just continuing the 
computation and producing bizarre results.

## Hermeticity

Since I always have a bias to isolation, even when it makes some things harder,
I really liked the concept of hermeticity applied on the language.

That implies having no support for things like environment variables:

```
Jsonnet programs are pure computations, which have no side-effects
and which depend only on the values which were explicitly passed.

In particular, the behavior is independent from the setup of the
system on which it runs (operating system, environment variables,
filesystem, …).

The semantics are defined almost entirely in mathematical terms,
with a few exceptions, where Jsonnet depends on well-established
portable standards (such as IEEE754 and Unicode).
```

They push for the idea of self contained configuration, but sometimes that
is not possible (like handling secrets), for that TLA's are provided.
They stand for Top Level Arguments and are very simple command line 
arguments, defined and used in the same way you would do when
calling a function.

For something that resembles more global/environment variables it has
external variables, which are accessible everywhere like env vars,
but with the clear distinction that they reside on their own explicit
namespace + they are immutable.
