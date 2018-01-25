# The Practice of Programming

## Dependencies

On chapter 3 they a markov chain algorithm is implemented
using C++ STL deque. The implementation using deque was
10 times slower than a list on windows (the list can
behave as a deque), on other platforms the behavior
was normal.

This prompted some reasoning about how we depend on other
software to build our own:

```
Less clear, however, is how to assess the loss of control and insight when the
pile of system-supplied code gets so big that one no longer knows what's going on under­
neath. This is the case with the STL version; its performance is unpredictable and
there is no easy way to address that. One immature implementation we used
needed to be repaired before it would run our program. Few of us
have the resources or the energy to track down such problems and fix them.

This is a pervasive and growing concern in software: as libraries, interfaces, and
tools become more complicated, they become
less understood and less controllable. When everything works,
rich programming environments can be very productive, but
when they fail, there is little recourse. Indeed, we may not even realize that
some­ thing is wrong if the problems involve performance or subtle logic errors.
```

Today software seems more an exercise on which frameworks and libraries
are you going to use instead of focusing on the problem at hand
and the design of your solution. Pushing these decisions further on development
opens space to depending on less stuff, and usually stuff that is simpler,
or not depending on anything at all
(just a good language and basic data structures).

Going from depending on everything to nothing does not seem like a sensible
choice, the problem is that the common industry behavior is more prone to
depend on everything and just hope for the best (Luck Driven Development).

## Prototyping

On chapter 4 when talking about interface design (not GUI, interfaces between
different software components) the approach used is to first build a
prototype that does not have a great interface, understand the problem better
and then design a proper interface.

There is a quote from Fred Brooks (from the book The Mythical Man Month):

```
Plan to throw one away, you will anyhow
```

When you are solving a new problem it is fundamental to get
insight about the problem and the solution space, prototyping is
a great way to do that.

## Designing Interfaces

* Hide implementation details
* Small and orthogonal
* Avoid creating temporary files/dirs (side effects)
* Avoid global state (side effects again =P)
* In general avoid surprises on your interfaces implementation (side effects as usual)
* Be consistent, avoid multiple ways of doing the same way
* Be consistent, in naming/parameters it can go a long way

## Error Handling

* Detect errors at a low level, handle them at a high level.
* Make error messages as clear as possible
* Use exceptions for exceptional situations only (or no exceptions at all =D)

It is kinda sad how errors are handled in the industry, they are usually
treated as second class citizens, they are so "not part of your logic"
that they even require a separate flow for them (exceptions), which is
the flow that no ones gives a fuck.

Not sure if 100% manual handling of errors is the way to go, but
right now I like the idea to force thinking and programming
errors as any other aspect of your logic, it requires design
and explicit thought, it is not that annoying thing that is
not part of your logic.

## Debugging

On debugging the book makes me pretty happy =). I always used debuggers
only to collect stack traces of where the failure occurred, from there
I usually use print statements or try to build a mental model of the
software so I can think about possible causes for the problem. Always
thought that I was lazy/stupid for doing like this (never learned to
use a debugger properly).

It is good to find smart people that does things like you, you feel
less stupid:

```
As a personal choice, we tend not to use debuggers beyond getting a stack trace or
the value of a variable or two. One reason is that it is easy to get lost in details of
complicated data structures and control flow; we find stepping through a program less
productive than thinking harder and adding output statements and self-checking code
a1 critical places. Clicking over statements takes longer than scanning the output of
judiciously-placed displays. It takes less time to decide where to put print statements
than to single-step to the critical section of code, even assuming we know where that
is. More important, debugging statements stay with the program; debugger sessions
are transient.
```

Perhaps I got lucky, my laziness is a lot like being smart =P.
It is not just the details of manipulating the debugger, even on
rich environments like eclipse + java the debugger has so much
information that it is much easier to thinking about your code.

The main advantage of doing this away from a debugger is that you
can do this away from a computer. Specially for hard problems
stepping away from the computer always seems to help me.

Another great example on the book of this:

```
Study the numerology of failures. Sometimes a pattern in the numerology of failing
examples gives a clue that focuses the search. We found some spelling mistakes in a
newly written section of this book, where occasional letters had simply disappeared.
This was mystifying. The text had been created by cutting and pasting from another
file. so it seemed possible that something was wrong with the cut or paste commands
in the text editor. But where to start looking for the problem? For clues we looked at
the data, and noticed that the missing characters seemed uniformly distributed through
the text. We measured the intervals and found that the distance between dropped
characters was always 1023 bytes, a suspiciously non-random value. A search
through the editor source code for numbers near 1024 found a couple of candidates.
One of those was in new code, so we examined that first, and the bug was easy to
spot, a classic off-by-one error where a null byte overwrote the last character in a
1024-byte buffer.

Studying the patterns of numbers related to the failure pointed us right at the bug.
Elapsed time? A couple of minutes of mystification, five minutes of looking at the
data to discover the pattern of missing characters, a minute to search for likely places
to fix, and another minute to identify and eliminate the bug. This one would have
been hopeless to find with a debugger, since it involved two multiprocess programs,
driven by mouse clicks. communicating through a file system.
```

Tooling will not save your ass all the time, great mental models
and good thinking will. Not that tools and debugger can't help,
but all what they can do is this, help. The main solution
is inside your head.

It is pretty much like diagnostic medicine, you need the ability
to given symptoms imagine all the possible causes. This will aid
you to choose which exams you need to make (which tools to use).

Without this ability a medic would be aimlessly asking for all
possible exams all the time, so will you if you just rely on
debugger to solve bugs.

## Testing

The book is very through on testing, including lots of cool examples
like how to test **memset** in C or probabilistic algorithms like a
markov chain. The idea is to test early and continuously.
Leaving to test in a big bang fashion in the end is not a good idea.

There is some attention to the fact that your tests may be broken,
the idea of writing the tests before and watching they fail aids
to catch buggy tests since they will not fail or fail for the
wrong reasons (this idea is not mentioned in the book actually).

There is a great deal of attention to what to test. Basically
thinking about boundary testing will help catch a lot of problems.
Great emphasis is given to write hard cases tests, focusing only
on normal flow use cases is just mildly useful since the idea
of the tests is to find bugs (like Dijkstra said, testing can
only prove the presence of bugs, not they're absence), so every
time you find a bug with a test you should feel good about
yourself.

There is a very cool quote from Donald Knuth:

```
To quote Don Knuth describing how he creates tests for the
TEX formatter, "I get into the meanest, nastiest frame of
mind that I can manage, and I write the nastiest [testing]
code I can think of; then I turn around and embed that in
even nastier constructions that are almost obscene."

The reason for testing is to find bugs, not to declare the
program working. Therefore the tests should be tough, and
when they find problems,
that is a vindication of your methods, not a cause for alarm.
```

On stress testing:

```
High volumes of machine-generated input are
another effective testing technique.
Machine-generated input stresses programs
differently than input written by people
does.

Higher volume in itself tends to break things because
very large inputs cause overflow of input buffers, arrays,
and counters. and are effective at finding unchecked
fixed-size storage within a program.

People tend to avoid "impossible" cases like
empty inputs or input that is out of order or out of range,
and are unlikely to create
very long names or huge data values.
```

Which is similar in spirit to the idea of being nasty and just
inputing crap to your application to see if it is
robust and will recover or if it will crash into a million
pieces.

What about the cases where it is very hard or even impossible
to create the expected output to validate in the test ?

There are two interesting ideas presented on this case
for the markov algorithm example.

One is to test conservation, check for properties that
the output must have instead of checking for a specific output.

The markov algorithm is used to create a text scrambler,
since it has randomness embedded on it you can't expect an
exact output, but every word on the output must be present
on the input, so you can test this conservation property.

Another property of the markov algorithm is statistical,
they also tested statistical properties of the output like
the likelihood of words appearing on the output.

The awesome part is that both approaches found bugs on
the markov chain algorithm they wrote (two very experienced
developers wrote buggy code), so it is not an academic exercise
to satisfy the ego.

On who should test code:

```
It is important to test your own code: don't assume
that some testing organization or user will find things for you.
```

Sad that more than 20 years later this principle is still
largely ignored. If developers like
[Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike),
[Brian Kernighan](https://en.wikipedia.org/wiki/Brian_Kernighan)
and [Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth)
need to write tests to find bugs on their code
so should you.

Rob Pike and Brian Kernighan where involved on developing Unix,
which was a product of Bell Labs, so these examples are not theoretical
on how software should be done on the ideal world, they are
real world examples on how to write software that will be
sold as a product.

If you feel overwhelmed by testing just remember:

```
The single most important rule of testing is to do it.
```

The only wrong thing you can do is to do nothing.
