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
