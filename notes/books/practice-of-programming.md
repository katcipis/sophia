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
