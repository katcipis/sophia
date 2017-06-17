# Good service

* Loose coupled
* Has high cohesion
* Has a good bounded context (domain is clear)


# Reasons to split

Why would you break a service in two or more ? Some reasons are presented.


## Pace of change

Usually when different parts of a system changes at different rates
it means that they handle considerably different aspects with different
customers in mind. Different customer means a different reason to change
that usually means a different pace of change.

An example is how fast a UI can change compared to a database schema.
Besides the different level of risk, the reasons for changing are quite
different too.

Given that difference, guaranteeing that changing one of them does not
affect the other makes totally sense. A good design can guarantee that
even if it is the same service/runtime. Separating on a different process
with a protocol around seems to just create a "physical" barrier around
this and enforce it. Which may be a good idea sometimes.

But beware, if a team can't design good modules and isolate stuff on code
they will probably do a lousy job defining good service integration protocols.


## Technology

You will get much more advantage if you solve some part of your problem
with Lisp, but the current service is written in Python. This is a hint
that it may be a good idea to handle this new problem as a different service
with its own runtime.

From my own experience, developing bindings between different languages
runtimes is nasty work that is best avoided, makes sense. You can of course
just spawn a local process from the Python code, if it makes sense.

But in a sense is a different service, with a protocol around it. Even a
binding between languages is a form of that.


## Scalability

Everyone loves to scale, even when they don't have too.
With different services it is trivial to scale only the parts of the system
that are really needing it, instead of just scaling the same monolith
that in theory loads a lot of resources when scaled.

Emphasis on the "in theory", if you have a really lazy service scaling it
should just use resources of the parts of the system that are being requested.

For example, think about features A, B, C. If you separate in three services
you can scale them separately. But putting the three of them on the same
service does not mean that when I scale the service it will waste up a lot
of resources, specially if it allocates resources lazily, only when requested.


## Security

Some part of the system has different security concerns from other, breaking
in more services can make things easier and make more explicit the ones
that have more security relaxed constraints.


# Service Integration

## Good integration protocol

* Tech agnostic
* Easy to use
* Can evolve without breaking current clients
* Hides implementation details (No OO/language details)
* Basically think on how your protocol will respond to change
* Also think about something really boring and ubiquitous (like http + JSON)


## Database Integration

Using the database to integrate multiple services

* No cohesion (usually logic around the data is spread across multiple services)
* To avoid duplication you can put logic inside the database...at your own risk
* High coupling (all services tied up on the same schema)
* Hard to perform non breaking changes (lack of orthogonality)
* If you have more than one service writing data you cant trust the data
* Mixed concerns, you have protocols/integrations and data/queries sitting together (good modeling can avoid this, never saw that although :-)


# Testing

* Avoid end to end tests (hard to be deterministic or fast, flaky and brittle)
* Use [Consumer-Driven Contract](http://martinfowler.com/articles/consumerDrivenContracts.html) to avoid end to end tests
* End to end tests responsibility is collective
* Smoke tests on production environment with [Blue/Green deployment](http://martinfowler.com/bliki/BlueGreenDeployment.html)
* Stubbing a integrated service on the networks is a good idea (mountebank and toxyproxy)


# SOA and Microservices

Basically the same thing, same core concepts.
Microservices are SOA implemented right ;-).


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


# Resilience and failure recovery

* DiRT (Disaster Recovery Test)
* DiRT remindes me of Chaos Monkey set of tools :-)


## Good patterns

* Bulkheads
* Timeouts
* Circuit Breaker


# Related materials

* [Domain Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)
* [A Classic Introduction to SOA](http://dannorth.net/classic-soa/)
