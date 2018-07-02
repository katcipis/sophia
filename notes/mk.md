# Mk a successor to Make

This notes are based on my initial experience with mk and the article
* [Mk: a successor to make](http://doc.cat-v.org/bell_labs/mk/mk.pdf).

Make has changed a lot since the paper has been writen, even tough almost
all the upsides of using Mk over Make are still valid. Now I will briefly
ellaborate on a small list of the ones that got my attention and motivated
my move towards using mk as my main automation tool.

One main advantage of Mk is that its syntax is almost identical to Make, so
migrating to Mk is considerably easy, and with time you can reap other benefits.

If you want to try it on linux all you have to do is install
[plan9port](https://9fans.github.io/plan9port/).

## smaller and simpler

Make...specially GNU Make is MUCH bigger than mk.
This can only reflect on more possible bugs and features that I don't need.

Check [Make](https://www.gnu.org/software/make/manual/make.html) docs. It is well
documented but it is way too big for something as simple as basic automation
(the code can be seen [here](http://git.savannah.gnu.org/cgit/make.git/tree/) ).

## rc shell

The first one is the fact that the shell used on mk is
[rc](http://doc.cat-v.org/plan_9/4th_edition/papers/rc)
which at least for me is much better than Make shell.

Why rc is better would be a subject of a long brainstorming, suffice
to say that is smaller/simpler than bash like shells. Simplicity comes on
a lot of small good choices, like the exit status code of the previous command
being stored at a variable named **status** instead of some gibberish
on shell that I can't even rementer.

## evaluation of shell

The recipe of the mk target (which is a shell script basically) is evaluated
as a whole script, instead of how it is done on make where each line of the
recipe is evaluated as a separated script, resulting in stuff like that:

```make
sometarget:
    a=command1 && \
    b=command2 $a && \
    command3 $b
```

While on mk, since the whole recipe is just one script,
you can just run:

```make
sometarget:
    a=command1
    b=command2 $a
    command3 $b
```

## targets with attributes

The way Mk allows targets to have attributes seems better than how Make handles
specific targets configuration. An example is when a target is virtual, meaning that
it is not involved on the generation of any file on the filesystem. On Make you need to
do this:

```make
.PHONY: clean
clean:
    echo etc
```

As the list of targets of a Makefile grows bigger it will be harder to check
which targets are virtual and which are not. There is a lack of cohesion on the
definition of the target, all its properties are not defined within the target,
some stuff is defined elsewhere.

Since mk supports attributes on the targets, the example above can be written in Mk as:

```make
clean:V:
    echo etc
```

This way the target declaration is self contained, self explanatory...cohese. And there is
a lot of other interesting attributes for targets.

## better builtin variables

There is a lot of variables that both Make and Mk generates automatically for you
so they can be accessed during the execution of the recipe. Things like the
names of pre-requisites, or the name of the target that you are building.

A good example is when you have a metarule, like:

```make
%.c:
	echo $@.c
```

This is a Makefile that prints the name of all the targets that match the pattern "%.c".
All Make variables are cryptic, you can check it [here](https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html).

How about Mk ?

```make
%.c:
	echo $stem.c
```

Much better =). And other variables are like "newprereq", "prereq", instead of a lot of single
letter garbage.

