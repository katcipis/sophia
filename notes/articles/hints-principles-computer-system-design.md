# Hints and Principles for Computer System Design

## Specs

There is an abundance of awesome advice on how to specify computer systems,
it would be pointless to just summarize all here (also a great deal of it leans
towards math and I'm math dumb, so would not be able to give insight into it),
but there is a few thingsthat gets extra attention.

One example is how poorly remote objects were
specified in the past, hence why they failed in being a proper abstraction
for remote procedure calls, I remember reading something about the main reason
being that the error handling and the performance is way too different from a local
object in order for it to be successfully abstracted as a local object. And on this
paper I got extra confirmation for this, on leaky/bad specs Lampson states:

```
Errors or failures in the code mean that the code won’t satisfy the spec, unless the spec gives
it a way to report them. A common example is a synchronous API that makes the code look
local, fast and reliable even though it’s really remote, slow and flaky.
```
