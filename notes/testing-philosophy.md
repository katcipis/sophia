# A Philosophy for testing

## Terminology

Basically 0 focus on terminology/taxonomy.

From [](https://commandcenter.blogspot.com/2012/06/less-is-exponentially-more.html):

```
My late friend Alain Fournier once told me that he considered the lowest form
of academic work to be taxonomy. And you know what? Type hierarchies are just
taxonomy. You need to decide what piece goes in what box
```

Ellaborate on discussions like X vs Y

## Scope

Ellaborate on scope, testing as a subset of "feedback". The domain is vast, we are
going to focus on a smaller part of it.

## When not to test ?

Related to scope.

## Principles

Copy most from: [Programmer Test Principles](https://medium.com/@kentbeck_7670/programmer-test-principles-d01c064d7934)
Also [When TDD Doesn’t Matter: What Do You Value?](https://medium.com/pragmatic-programmers/when-tdd-doesnt-matter-what-do-you-value-91c628dc4488)

But extend with some other simple principles, like easy to run and with a uniform/easy
interface (like make test).

There is no (easy ? plausible?) way to avoid slow/non-deterministic test unless you have mostly useless tests.

Navigate the trade-off space.

Separate slower/non-deterministic from deterministic.
