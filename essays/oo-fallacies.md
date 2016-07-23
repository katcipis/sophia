This post is based on this [great talk Uncle Bob gave at Yale](http://som.yale.edu/news/2014/09/uncle-bob-martin-speaks-yale-som)
and some experiences I had in my life developing software in C/C++/Java.

# Disclaimer

I have no intention of proving that C is the best language on the world, neither prove that object orientation is bad
(this includes object oriented languages). 

I like object orientation and use it quite often. This is just a reality check :-). I learned to develop software using 
object orientation, so I'am strongly biased to over objectify everything :-). 

But as you continue to think about what you are doing and why, you see some fallacies. See this as an exercise to
become a little less biased towards excessive object orientation (also against transforming it on some kind
of religion, like OO will save us all from bad software).

If you see something very stupid on this post, bear in mind that I'am still learning :-).

# What new stuff did object orientation gave us ?


## Encapsulation

Makes it worse than C.


### Public / Private / Protected


## Inheritance


## Polymorphism


## A way to model the real world

This is the one that I always heard at university, with objects you will be able to represent the real world !!!

Wow, for sure this does sounds awesome, after all if my software represents the real world it will be easy
to understand and everything will just fit nicely.

The only problem is that it does not work that way.

If I got a cent every time I designed a awesome collection of interfaces and objects that interact like they
do in the real world but won't conform to what the client wants I would be rich :-).

Usually the client wants something that is not related at all with what the objects you designed do
in the real world (Uncle Bob uses shapes on his examples).

You put all this effort modeling the real world just to see it going down by one odd requirement.
I remember vividly the feeling of being upset by this kind of requirements, like it was breaking my
beautiful real world design.

This is extremely absurd, the software FOR the client, not for my desire to model the real world with objects :-).
Here enters the KISS (Keep It Simple Stupid) principle, and responsive design. Don't try to model the real world,
do the simplest thing that can possibly work and then deliver it to the client ASAP.

The client will give you feedback and new features to develop, then you will be able to find the better modeling
to his needs, not to the real world :-).

As Uncle Bob says, this real word thing is just marketing.

And after all, it can be easily done without object orientation.


# Conclusion

Safe polymorphism ;-), and made it easier.
