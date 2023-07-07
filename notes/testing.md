# Testing

Interest material on the topic of testing.

## [The History of JUnit and the Future of Testing with Kent Beck](https://www.se-radio.net/2010/09/episode-167-the-history-of-junit-and-the-future-of-testing-with-kent-beck/)

Demistifies a lot of dogma surrounding TDD, like you have to do it all the time,
the only way to get a good design is with testing (not true), etc.

Also debunks TDD as something different from BDD, as usual just a misunderstanding
and people getting to hung up on classification. Good stuff. It has very little to
do with actual JUnit, which is good =P.

## [Programmer Test Principles](https://medium.com/@kentbeck_7670/programmer-test-principles-d01c064d7934)

One of the best/short materials available to showcase how there is no right answers
on testing, just tradeoffs, and you should not marry with any particular
approaches to how test. Also elucidates a lot on the perils of over-mocking
and coupling too much into structure.

## Mocking

I usually don't like mocking, specially general purpose mocking interfaces since they are quite
clumsy to use IMHO and the error messages are usually cryptical since the test fails inside the
framework. For simple scenarios it may be OK, but it doesn't scale well (complexity + error messages).

And I'm not the only one :-)

### Tailscale/Apenwarr

You can see [here](https://github.com/tailscale/tailscale/blob/28ee355c56c68a446ecfdc755c67311349bf6a3c/ipn/ipnlocal/state_test.go#L278) he said:

> [apenwarr] Normally I'm not a fan of "mock" style tests, but the precise
> sequence of this state machine is so important for writing our multiple
> frontends, that it's worth validating it all in one place

It is a cool example of someone else (but much better than me) that has a similar distaste for mocks, but
using them when they are really necessary (last case).

And when he had to...he built his own...no generic/code generated one with a framework.
More details [here](https://github.com/tailscale/tailscale/blob/28ee355c56c68a446ecfdc755c67311349bf6a3c/ipn/ipnlocal/state_test.go#L94).

More examples of mocks on the tailscale repo (hand written):

* [magicsock](https://github.com/tailscale/tailscale/blob/28ee355c56c68a446ecfdc755c67311349bf6a3c/wgengine/magicsock/magicsock_test.go#L2672)
* [awsstore](https://github.com/tailscale/tailscale/blob/28ee355c56c68a446ecfdc755c67311349bf6a3c/ipn/store/awsstore/store_aws_test.go#L20)

At least when grepping for "mock" it doesn't seem the repo has too much of them..just a few, that is nice. Probably hard to
achieve such a design, but certainly desireable. And this is software that does a lot of networking, so quite challenging to
test in a way that doesn't need a lot of mocking/faking.

### Kent Beck

From [Programmer Test Principles](https://medium.com/@kentbeck_7670/programmer-test-principles-d01c064d7934):

> Structure-invariant tests requires a particular style of programming and design as
> well as a particular style of design. I frequently see tests that assert,
> “Assert that this object sends this message to that object with these parameters and
> then sends this other message to that other object.”
> An assertion like this is basically the world’s clumsiest programming language syntax.
> If I care about the order of operations, I’ve designed the system wrong.

It captures most of the bad feelings I have with mocks, the syntax is indeed very clumsy and focus
on the order of operations is also an unnecessary form of coupling. Of course sometimes the ordering
is fundamental..then go ahead and validate ordering...but general purpose mocking always seem to
validate ordering, specially because you need to return different values depending on the order etc.
When I use fakes I never have this problem, since it is not a general purpose programable mock,
and when I need some features more close to a mock, like inspecting N calls, it is not embedded
on the mock the ability to validate the calls or fail the test etc. 

Anyway, not sure Im right about this, just seems like I'm not the only one.
