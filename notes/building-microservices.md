# Good service

* Loose coupled
* Has high cohesion
* Has a good bounded context (domain is clear)

# Service Integration

## Checklist of good integration protocol

* Tech agnostic
* Can evolve without breaking current clients
* Hides implementation details
* Easy to use
* Basically think on how your protocol will respond to change


## Database Integration

Using the database to integrate multiple services

* No cohesion (usually logic around the data is spread across multiple services)
* High coupling (all services tied up on the same schema)
* Hard to perform non breaking changes
* If you have more than one service writing data you cant trust the data


# Testing

* Avoid end to end tests (hard to be deterministic or fast, flaky and brittle)
* Use [Consumer-Driven Contract](http://martinfowler.com/articles/consumerDrivenContracts.html) to avoid end to end tests
* End to end tests responsibility is collective
* Smoke tests on production environment with [Blue/Green deployment](http://martinfowler.com/bliki/BlueGreenDeployment.html)
* Stubbing a integrated service on the networks is a good idea (mountebank and toxyproxy)


# SOA and Microservices

Basically the same thing, same core concepts. Microservices are SOA implemented right ;-).


# Monitoring

* How we will cut costs if we don't have any idea of how the service is performing on production ?
* Good monitoring enables elasticity to cut costs
* Monitor for some time and perceive what is the *normal* behaviour of the system (cpu/mem/io usage)
* How to really know if the service is working ? [Semantic Monitoring](http://www.thoughtworks.com/pt/radar/techniques/semantic-monitoring)
* Semantic monitoring usually is made with synthetic transactions
* Monitor asynchronous operations with correlation ids
* Measure response time and error rates, also for downstream dependencies


# Related materials

* [Domain Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
* [A Classic Introduction to SOA](http://dannorth.net/classic-soa/)
