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
* Measure response time and error rates, also for downstream dependencies (circuit breaking)


# Security

Having smaller services gives you the advantage of building different security precautions to different services,
depending on the sensitivity of the information they hold, or on what they can do.

There is a series of approaches/tools you can take on security:

* [HMAC](https://en.wikipedia.org/wiki/Hash-based_message_authentication_code)
* [JWT](http://jwt.io/)
* [OpenID Connect](http://openid.net/connect/)
* [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)
* [Shibboleth](https://shibboleth.net/)
* [Gluu](http://www.gluu.org/)
* [OpenAM](https://en.wikipedia.org/wiki/OpenAM)
* Client Certificates
* Everything allowed on perimeter (main entrance gateway)
* Basic Auth on HTTPS 
* API Keys

It depends on the usage of your API. To provide services to a small set of partners, client certificates may be a good
idea. But for a great set of clients it would be terrible to manage, for example.


# Related materials

* [Domain Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
* [A Classic Introduction to SOA](http://dannorth.net/classic-soa/)
