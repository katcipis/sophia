# Testing

* Avoid end to end tests (hard to be deterministic or fast, flaky and brittle)
* Use [Consumer-Driven Contract](http://martinfowler.com/articles/consumerDrivenContracts.html) to avoid end to end tests
* End to end tests responsibility is collective
* Smoke tests on production environment with [Blue/Green deployment](http://martinfowler.com/bliki/BlueGreenDeployment.html)
* Stubbing a integrated service on the networks is a good idea (mountebank and toxyproxy)
