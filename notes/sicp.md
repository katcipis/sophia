# Structure and Interpretation of Computer Programs

## Framework for analysing languages

* Primitives
* Composition
* Abstraction

LISP examples:

* Primitive: 1
* Composition: (+ 1 4 (- 2 3))
* Abstraction: (define (myfunc x) (* xx))

All languages are formed by these three, always analyse a language through this lenses.

## Tackling complexity

There is 3 main ways to handle complexity:

* Black box abstraction
* Conventional (uniform) interfaces
* Making new languages (metalinguistic abstraction)

When discussing software engineering there is a great
deal of focus on black box abstraction. There is
less discussion on the greatness of finding good
uniform interfaces (like plan9 representing all
resources uniformly through a file interface).

There is even less discussion on building languages =(.

## Iteration VS Linear recursion

Giving some cool examples, it is presented the idea of iteration and linear recursion.
It does not have anything to do with using recursive functions on the language, it is
the nature of the algorithm.

Even using recursion, if the nature is iterative you will have a fixed O(1) space
complexity. If the nature is recursive, space complexity will be N (for some N,
depending on the algorithm).

It seems to me that the iteration version is what is optimized to tail recursion, so even
if you have a recursion, the space complexity is O(1). Linear recursion would be when
you cant tail optimize and the space complexity linearly (or worse) grows.

Iterative add:

```
(define (add x y) 
    (if (= x 0) (y) (add (-1+ x) (1+ y))))
```

Linear recursive add:

```
(define (add x y) 
    (if (= x 0) (y) (1+ (add (-1+ x) (y))))
```

Both are recursive definitions, but just one is a recursive procedure.
When you evaluate the linear recursive one, it will expand as big as X.
The iterative one has always the same size.

Iterative add:

```
(add 3 4)
(add 2 5)
(add 1 6)
(add 0 7)
7
```

Recursive add:

```
(add 3 4)
(1+ (add 2 4))
(1+ (1+ (add 1 4)))
(1+ (1+ (1+ (add 0 4))))
(1+ (1+ (1+ 4)))
(1+ (1+ 5))
(1+ 6)
7
```

## Operator overloading and the glory of prefixed operations

It seems that operator overloading is a good idea because of uniformity.
For native types like integers you can write:

```
1 + 2 + 3 + 4
```

But if I have a complex type (like complex numbers) and want to add them I would need to:

```
complex1.add(complex2).add(complex3).add(complex4)
```

Well, that is one way to do that, but without operator overloading you cant escape from
losing uniformity on your code. Operator overloading allows you to:

```
complex1 + complex2 + complex3 + complex4
```

But on a prefixed (and already uniform) language like lisp you have:

```
+ 1 2 3 4
```

It is already cool because you dont have to repeat the operator. But now if you want to
add complex you can just define a new function (the operator is also just a function on lisp):

```
add complex1 complex2 complex3 complex4
```

Its a interesting win for prefixed operations. Symmetry is beautiful.

## Naming Spirits

Since the course has a lot of metaphors with programming being like magic,
where you write magical spells and command them to be executed, giving life
to things, it is expected that more metaphors related to magic would appear.

A great one is the idea of give names to spirits. It is common knowledge
that if you know a spirit name you have control over it. Missing concepts on
the code (usually abstractions, like functions or data structures) are like spirits
spread through your code, you have no power over them. Extracting them and giving
them a name gives you control over them.

It is a nice metaphor to extract functions and create abstractions that helps
to solve more than one problem.

For example, when implementing the heron's method to calculate square roots
if we do not identify the fixed point spirit by name and extract it
as a separated function from the square root calculation we would be
unable to use it on other functions that also needs a fixed point.

Besides reuse of code, we would have a hard time identifying that
the heron's method uses fixed point to find the solution, it would be
implicit. So even when reuse is not in play, extracting these spirits
from code and naming them can make things more clear.

What is interesting is that you can't escape the spirit, if you do
not name it, it will still be there, but nameless, uncontrolled, rampaging
through your code.

## Generic Operators

On lecture [4B](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-001-structure-and-interpretation-of-computer-programs-spring-2005/video-lectures/4b-generic-operators/) Hal Abelson brilliantly presents a very simple problem (representing complex numbers) and how
to add generic operators (like add and mul) that works transparently independent from how the
complex numbers are implemented (using polar values of the real/imaginary one). The problem is extended
to even operating on polynomials.

At the start of the problem he shows generic operators that basically does some ifs
and operates differently depending on the type of the complex number. He shows that
this design is not a good one because it does not accomodate change well, since everytime
you want to add some new representation of a complex number on the system you need to change
some central code that does some if'ing to check the type (he compares it with some manager
that only does bookeeping =P).

To solve that he introduces the notion of a type system registry. Basically when you
add a new type you also use functions like this:

```
(put 'typename 'functionname functionimplementation)
```

And the generic algorithm are going to get the type of the instance and use it
to find a proper implementation of a function it needs...if that type has a function
registered to it.

He mentions that you could bypass this central type registry completely if each instance
had this map of functions with it, he calls it "messaging". It seems to me that this is
what they understood as object orientation at the time.

The idea seems very appealing to me because it does not require any kind of central type
registry where all types of your program are registered, and it also makes the type of
objects (data structures, or whatever you are passing around on your system) completely
irrelevant. You can just send a message or even query if a message is supported at runtime.

The disadvantage seems to be performance, specially because there will be a LOT of copies of
functions depending on the system you are implementing. When I thought of that I remembered that
copying is the mechanism that life itself uses to solve this, each cell is independent and has
a complete copy of all the functions it can perform, there is no prototype or central type
where the functions are stored.

It seems that nature chose this way and it solved the space problem
with DNA that is simply brilliant on how it stores information. Perhaps we are stuck with bad
models for lack of better ways to support duplication (how we represent information),
or just lack of hardware or nature is wrong =P. Although it is undeniable that life scales
on order of magnitudes that we can't even
compreehend and we are not even close on building something that scales even near as good.


## Assignment, State, and Side-effects

On lecture [5A](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-001-structure-and-interpretation-of-computer-programs-spring-2005/video-lectures/5a-assignment-state-and-side-effects/) Gerald Jay Sussman introduces the **set!** function, that allows
changing the value associated with a variable. Basically he introduces the concept of
assigment, effectively changing state. Being able to assign new values to the same variable +
scoping (closures) create the same state manipulation mechanism on todays object oriented
languages, with the same perils.

The lecture has a great explanation of the perils of side effects, the lost of the "functionallity" of
the procedures, like:

```
(rand)
(rand)
```

Not producing the same result, hence not being functions anymore.
But the best part is that he presents a very interesting problem, aproximating pi.
The algorithm involves a mounte carlo simulation, that uses randomic numbers.

He shows an implementation where there are side effects on funcions (not purely functional) and
one without side effects. The one with side effects was able to encapsulate the random number
generation inside one place, the mounte carlo algorithm was not even aware that there where
randomic numbers envolved on the experiment (mounte carlo here is used to create a statistical
sampling on top of an experiment).

The functional approach without side effects require the seed of the random number generation to
be retro feed on the random function, since the function is pure and don't keep any state. This
generated the need to expose this parameter on all the APIs, including the mounte carlo one, since
it was called on a loop the loop also need to feed the new parameter for the random number generation.

So in the end the detail of random number generation leaked through all the code, when with state manipulation
we where able to hide it. In the end he is still on doubt if assignments and state manipulation is worth the
trouble...but it is refreshing to see a intelligent perspective (balancing tradeoffs) on the subject
instead of todays hipe.


## Cool Quick Stuff

* High order functions as a way to separate concerns, isolate changes, avoid duplication and express patterns (so much for OO)
* I found another human being that dislikes mathematical notation as me, got happy :-)
* Abstractions is a way to apply divide and conquer

## Interesting tasks

* Implement sieve of Eratosthenes
* Implement square root approximation algorithm
* SAT-3 checker
