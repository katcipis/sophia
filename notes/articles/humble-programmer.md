# Humble Programmer

The article does a great introduction to the concept
of software being "intelectually manageable" which is a cool
way to say "I'm able to understand this shit".

As usual the more recorrent subject on software quality
is the ability to understand code that has been already written.
Writing a lot of messy code is easy, developing something easy
to read and understand is hard.

It raises the issue if software complexity must grow exponentially
or linearly with the size of the problem. On Dijkstra opnion it is
possible to be linear, and the only way to do that is to factor
code correctly, so our limited minds can grasp the complexity of
software on one piece of a time.

A key piece of this is abstraction. The purpose of abstraction is
not be vague, but to create a new semantic level and be precise
on this new semantic level. This kind of isolation from other
details is what enables us to get a good grip on each part of
the solution to a possible big problem, the way to scale 
software development.  Good abstractions are crucial.

This is important because layers and abstraction also have
developed a bad reputation thanks to over enginered object
oriented designs.

Another cool aspect of the article is the obvious idea that
we are very limited, phrases like:

```
The competent programmer is fully aware of the
strictly limited size of his own skull.
```

One of our limitations is the tool we use to express
our thoughts, the language we use, that is why the choice
of language is by far the most important one, the ways
you can conceive a solution to a problem is limited by
the language you are using. The expressiveness of the language
will limit how far you can think on solutions to the problem.

Sometimes we think it is not like that, we like to think that
we are awesome, but we are not, the language you choose will
limit you. That is another way to be humble, realize that
and search for good modest languages to develop in, they
are going to enable you, or limit you.

An interesting point of view is how can we expect to tame the
complexity of our code if we do not even tame the complexity of the
language chosen for the task:

```
I absolutely fail to see how we can keep our growing programs
firmly within our intellectual grip when by its sheer baroqueness
the programming language —our basic tool, mind you!— already escapes
our intellectual control.
```

It is interesting that in projects that uses languages that are not
feasible to be tamed because of their sheer complexity usually the
problem is tackled with guidelines and enforcing just a subset of the
language, the subset that is intelectually manageable and that will
also keep your code intelectually manageable. But this effort is
wasted time, the language should not be so complex that finding the
correct subset of it is an integral part of the problem.

Now for some fun =):

```
And if I have to describe the influence PL/1 can have on its users,
the closest metaphor that comes to my mind is that of a drug.

I remember from a symposium on higher level programming language a
lecture given in defense of PL/1 by a man who described himself as one
of its devoted users. But within a one-hour lecture in praise of PL/1.

he managed to ask for the addition of about fifty new “features”, little
supposing that the main source of his problems could very well be that it
contained already far too many “features”. The speaker displayed all the
depressing symptoms of addiction, reduced as he was to the state of mental
stagnation in which he could only ask for more, more, more...

When FORTRAN has been called an infantile disorder, full PL/1, with its
growth characteristics of a dangerous tumor, could turn out to be a fatal disease.
```

Even hardware can impose models and ways of thinking that can
empower you or hinder your ability to understand harder problems:

```
The reason that I have paid the above attention to the hardware
scene is because I have the feeling that one of the most important
aspects of any computing tool is its influence on the thinking habits
of those that try to use it, and because I have reasons to believe that
that influence is many times stronger than is commonly assumed.
```

The idea that we transcend our tools is misplaced when it comes to
computing, perhaps this can change with time but the way we think
today on how to code is even tied to details of how the hardware
itself works (think paralelism for example). He puts this idea on a
very clear way:

```
I observe a cultural tradition, which in all probability has its
roots in the Renaissance, to ignore this influence, to regard the
human mind as the supreme and autonomous master of its artefacts.

But if I start to analyse the thinking habits of myself and of my
fellow human beings, I come, whether I like it or not, to a completely
different conclusion, viz. that the tools we are trying to use and the
language or notation we are using to express or record our thoughts,
are the major factors determining what we can think or express at all!
```

An excelent guidelines for developing tools and languages in general is:

```
brainpower is by far our scarcest resource
```

I think this is an essential shift on how we used to develop tools.
Instead of thinking on what is possible with a computer we need to
think on what is possible for a human being to understand given our
limitations. This is not an invitation to mediocrity, but it is important
to acknowledge the limitations of our brains.

The action of designing is to impose restrictions, the restrictions should
optmize for human cognition (and its limitations).

Programming is hard, approach it with respect.
