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
through your code (here rampage would be duplication =P).

## Cool Quick Stuff

* High order functions as a way to separate concerns, isolate changes, avoid duplication and express patterns (so much for OO)
* I found another human being that dislikes mathematical notation as me, got happy :-)
* Abstractions is a way to apply divide and conquer

## Interesting tasks

* Implement sieve of Eratosthenes
* Implement square root approximation algorithm
* SAT-3 checker
