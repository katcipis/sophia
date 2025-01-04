# Additive Programming

The core idea of the book is Additive Programming, basically it is a guiding principle to build systems that can evolve in time but without breaking and without requiring full rewrites.

The 3 main goals when designing software like this is:

* Generality: Simple/general abstractions that can be used as is for new features (no changes)
* Evolvability: New features that require new code/abstractions should be added to current software, without having to change code that supports other features (kinda like the Open/Closed principle from SOLID I suppose)
* Robustness: Adding the new features (by addition, not changing) doesn't break the old features (the orthogonality/isolation in the design)

None of that is new but it is refreshing to read about this. Most of the current design advice for systems design is around giving up on trying to do any design.
The current trend in software design is "microservices" which advocates that the design principle for your system is just "make it small"....if everything is just
the composition of very small things you're going to be fine. 

Contrast this with the design principles advocated in the book. You need to find proper abstractions that are composable and general enough.
This is very hard work..and it is domain specific..there is no magic recipe that guarantees you found these boundaries/abstractions...just "making it small" doesn't solve it,
if you get the boundary wrong and split something in half that you shouldn't you're going to have a bad time, but the industry is addicted to "simple answers", thinking is expensive,
not thinking is cheap. Reminds me of the old adage "if you think proper architecture is expensive you should try the wrong architecture".

The author pushes towards being able to find good abstractions/generality and that is cheaper than just rewriting software every single time requirements change.
The whole approach of just "make code disposable" is built around the idea that you don't know the future, so you can program for it.
It kinda makes sense, you really don't know the future, but it makes the mistake of assuming that it is impossible, or too expensive, to make a design that can adapt/evolve.
Sometimes it is OK to just throw off stuff, and making it easier to do this can be a good idea in some cases, but doing that all the time as a "design principle" is stupid,
because that is the absence of design in general, just don't do any design and rewrite stuff when it breaks. You don't invest in design when you have no idea of what you are building,
like a prototype, but in most cases you do have some good idea of what you are building and if you spend some time thinking you will find some primitives/abstractions and some ways of
composition that makes sense in your domain.

# Architectural Parti

I liked the concept of an architectural parti: https://en.wikipedia.org/wiki/Parti_(architecture)
I think I did some very similar things along my career as you try to specify systems and communicate ideas.
For software depending on the granularity level that you are discussing design this will take different forms.
Quoting:

```
In programming, the parti is the abstract plan for the computations to be performed.
At a small scale the parti may be an abstract algorithm and data structure description.
In larger systems it is an abstract composition of phases and parallel branches of a computation.
```

I recently did all of those in different discussions at work. The abstraction algorithm is usually pseudo code, used to discuss more formally how something will be made.
Data structure descriptions are done all the time when we are specifying (usually first) data that will be received/sent on APIs or on a dataflow pipeline (like using events).
The abstract composition of phases is usually a more high-level data flow diagram, especially if you have an "event oriented" system, although I prefer to see this as a dataflow/streaming
system that talks about "event sourcing". Independent of how you call it, it is quite useful to discuss new features to have a sketch of how the data will flow around the system and how
some new data will be added/processed. I did this for a long time without having a cool name for it :-). Now I can say I will do an architectural parti and feel fancy about it.

# Robustness: Postel Law

I always liked Postel's law:

"Be conservative in what you do and liberal in what you accept"

But the author gives a new perspective on this by using an analogy with digital systems design. When designing such systems it is a good idea to always reduce noise in the system,
but making the system robust to perturbations on the signals. To do that you apply Postel's law, each component in the system is very liberal in the inputs it accepts,
accepting more than it should and finding ways to work with such noise information, but then being very precise/strict with the outputs it generates.
If every single component in a complex pipeline does that you have a system that always reduces noises/failures instead of amplifying it.

Now you can imagine a dataflow system, like some digital system that interconnects multiple components, where each component can handle perturbations on the input but
always produce a more well behaved/corrected output, each component always tries to improve whatever is happening on the system.
So new information being processed has a chance of being processed with success instead of causing failures that need human intervention (evolvability).

Specially for high scale/complex systems I can't imagine a different way to build things, it is how something as anarchic as the internet can work, people won't
understand specifications the exact same way, so be liberal with perturbations on input data but be well behaved/strict on what you produce.

This does create overhead, code that does that is bigger/more complex, it is a trade-off. But in some scenarios it is worth it to pay for this.
The alternative is to keep rewriting code and for the system to fail when any little perturbation/change happens on data.

# Extending Software by Dynamic Dispatch

Instead of using the classical object oriented approach of subclassses/hierarchies as a way to extend the system and add new behavior a different approach is proposed.
It leans towards operators/function overloading. Basically instead of having a simple system where a name maps to a single function, like

* add -> function that adds

Or something like it but where the object/type works as a namespace:

* Type.add -> function that adds for that type

You get a more general/flexible system that is decoupled from the types definition. Quoting directly:

```
However, unlike the generic dispatch in the typical object-oriented programming context,
our generic dispatch doesn't involve ideas like classes, instances and inheritance.
These weaken the separation of concerns by introducing spurious ontological commitments.
```

A dispatching engine that works like this:

* name + <predicate function> -> handler

Where the <predicate function> is a function that returns true if it can handle the given args or false otherwise. This is much more powerful than inheritance as a
means of extension/composition, but has a considerably high cost since the dispatching logic can be as sophisticated as you want
(the predicate function that checks if args can be handled can be arbitrarily complex).

It is an interesting idea that I would like to explore more someday. The dispatch mechanism provided in some languages as Erlang using pattern matching is a subset
of this implemented in the language runtime, going even further and building a system where you can even define your own matching on the dispatch is super powerful,
but it does introduce some challenges...especially when the matching lacks no constraints at all (arbitrary functions).

One challenge is guaranteeing that the predicate functions are disjointed, ideally there should be only one handler per set of args, but the author talks about
being necessary to handle when there is more than one match, the final system is not trivial and I imagine that debugging this
dispatch logic may get nasty in a big system... But it does build a more flexible/power system than classical object orientation where you need to understand the
structure/ontology of types before hand.

It is like having addition for numbers and then adding additional support for vectors and matrices but without having to touch the vectors/matrices code.
It is truly "additive", you just add new support on your newly written code but you don't need to go around changing anything on the current types
(the way it would be done with object oriented stuff, where now you need to add new methods on every single type to support something new).

It is similar to just creating new functions that operate on those types but with a general purpose mechanism, so algorithms that use addition to work on
numbers will also just work out of the box for vectors/matrices.

The second challenge would be that some operations have different properties for different types and the predicate is not enough to solve this problem.
The classic example being multiplication and matrices, multiplication no numbers is commutative but it is not commutative for matrices, so algorithms that
count on multiplication being commutative will break when the args are matrices (but that is abstracted away, creating a nasty bug to debug).

Another major challenge is that this approach is hard to be implemented with robustness in the sense that adding a new handler/predicate may break old code.
The classical example for operator overloading being you overloading addition for numbers but doing something wrong there, now any number addition being done
in your system is broken, while trying to add feature X you broke everything else. It is really hard to have powerful metaprogramming in a language and not
open the door to something like this, so far the author didn't provide a solution for this and it seems like a trade-off between the ability to
generalize/extend software VS robustness/stability.

It still should be pretty easy to just undo your changes, remove the new code. While the classic "refactor/change things everywhere" will be more messy to
undo depending on how many other changes happening after you did a refactor and added some new feature.
