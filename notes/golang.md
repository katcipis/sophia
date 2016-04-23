# Golang

## Odd Stuff

### Method Sets

* [Method Sets](https://github.com/golang/go/wiki/MethodSets)

Why I cant reference maps and interfaces ?

Because of that Golang performs a syntatic sugar when you call pointer methods 
on a value object you dont know that on the start.

If you have a method that has a pointer receiver:

    func (self *MyType) Method()

And you call the method using a value object:

    a := MyType{}
    a.Method()

This cant actually work, a is a value object and its method set does
not have **Method**. But this will actually compile and work because Go does:

    (&a).Method()

You have problems when you try to pass the value object as an interface.
It does not have the method on its method set and interfaces cant be referenced.
That is why your build will fail (Go cant do &(obj).Method() if obj is an interface).


### Channel Behaviour

* [Nil channel blocks](http://dave.cheney.net/2014/03/19/channel-axioms)

Why the hell should it block instead of panicking ?

My problem with channels is the lack of symmetry/uniformity on the behaviour.


#### Nil channels

These ones are symmetric:

* Read from nil channel blocks
* write to nil channel blocks

This one is pretty symmetric, so its ok, the problem arises with closed channels.
It seems that both kind of channels are invalid (nil and closed ones), but this idea
may not be true, it just an impression that I have.


##### Closed channels

* Read from closed channel returns zero value
* Write to closed channel panics

This one is pretty confusing. There is no blocking behaviour at all, and no uniformity between
read/write at all (like nil channels).

But after some reading, this behaviour is pretty useful to use a pattern where only one goroutine
writes stuff and N other goroutines are reading from it. When there is no more jobs to be done
closing the channel will "broadcast" nils to all readers (since read returns nil).

Care must be taken with closing, since it will panic. This obligates you to model your application to
always have only one responsible to close channels, which to be honest seems to be a good idea.


##### Select behaviour

Read from nil channel wont block on a select that has a default clause (default is called if all channels are nil).
This happens because select never blocks on a channel that is not ready, since a nil channel will never be ready
it will never be selected.

This is used to implement the logic of setting a channel to nil when you want to disable a case on a select.
It makes it easy to implement some complex logic, but for someone new to Go it is very hard to understand that
setting to nil will disable the case (at least for me it was, but only on the beginning).


##### More info about it

* [why chan nil blocks?](https://groups.google.com/forum/#!topic/golang-nuts/QltQ0nd9HvE)
* [why write to a closed channel will panic?](https://groups.google.com/forum/#!topic/golang-nuts/1wgT_90Q1dk)
