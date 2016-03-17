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

My problem with channels is the lack of symmetry/uniformity on the behaviour. Basically:

* Read from nil channel blocks
* write to nil channel blocks
* Read from closed channel returns zero value
* Write to closed channel panics
* Read from nil channel wont block on a select that has a default clause (default is called if all channels are nil)

Some info about it:

* [why chan nil blocks?](https://groups.google.com/forum/#!topic/golang-nuts/QltQ0nd9HvE)
* [why write to a closed channel will panic?](https://groups.google.com/forum/#!topic/golang-nuts/1wgT_90Q1dk)
