# Docker Compose

Why a note on docker-compose ? Well I had a very long history
of love/hate with it, actually not love with it, but love with the
idea of easily setup test environments to use on integration
tests. I used it for integrating stuff with RabbitMQ and PostgreSQL
for example, in a reasonably easy manner (at least on v1).

There is a lot of approaches to run integration tests, but as time 
and versions progressed it seems to me that docker-compose only became
a worse and worse option for testing, specially when it is used only
for testing purposes (my case).

The final blow was [this issue](https://github.com/docker/compose/issues/3492),
when they just broke docker-compose run for the sake of their own
reasons, which are unrelated to testing environments, but related to
their own deployment related features, from
[this comment](https://github.com/docker/compose/issues/3492#issuecomment-230931596):

```
It definitely shouldn't be the default behaviour of run to add the aliases,
because then you'd get situations where one-off containers are accessible via
the same hostname as long-running containers for the same service,
and part of the load-balancing group - which is almost never what you want.
```

Which kinda consolidates how the tool evolved to be a deployment tool.
Maybe that was always the objective, but on v1 it was much smaller and
it felt more like a good fit for reasonably simple testing environments.

Now looking at the docs for v3 is daunting, both the cli reference as the
spec for the config files, it is riddled with features and a lot of the
features are marked as suitable only for deployment and even relates to
docker swarm. So in the end what felt like an approach for dev envs that
requires spinning up multiple services locally now seems unsuitable for
it given complexity. Both the spec/docs are bloated and actually
running your tests got harder.

Yes, it is still possible, but it only got worse for that scenario and
maybe that is not the only way to run integration tests. So I wrote this
as a reminder on why I started steering away from using docker-compose
for dev envs / testing and started looking for alternatives.

# Alternatives

One tool that is more like docker-compose, but at least is built
on top of Kubernetes, which them would give you advantages on
uniformity if you use Kubernetes in production is
[Skaffold](https://skaffold.dev/). It doesn't seem particularly
lightweight, but it does have the advantage that it is a tool
explicitly for "local development", so hopefully it won't be clobbered
with deployment/prod related features like docker-compose.

Another interesting idea is to build the test environment programmatically
by spinning up containers and linking them together directly from code,
like the [testcontainer-go](https://github.com/testcontainers/testcontainers-go),
which has the advantage of defining environments and reusing them using
actual code instead of YAML and also enables you to build isolated environments
with proper cleanup per individual test instead of an entire test case.

The reuse side of it is very interesting, if everything in an organization
works with Go you could provide Go libraries that build test environments,
that is much nicer/easier than avoiding duplication of docker-compose YAML files.
