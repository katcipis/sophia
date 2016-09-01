# The Go learning experience

## How to learn Go ?

### The good

### The bad

#### Vendoring

#### Method Sets

#### Channels asymmetrical behaviour


## Practices


### Testing

* Table driven tests (how they make more clear it is the same test with diff input/output)
* Setup/Teardown using functions
* Build tags in tests
* Parallelism problems and shared state on tests

TODO: Talk about the problem with each package executing in parallel (-p) thing


### Coverage


### Static analysis


### Error handling

All alternatives besides error handling through abstractions results on lost of capacity to do proper dependency inversion

http://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully


### Naming receivers


* Dont use self / this
* Name variables according to interface being implemented
* Methods on Go are just syntatic sugar for calling functions
* Also a way to implement interfaces (protocols)

https://blog.heroku.com/neither-self-nor-this-receivers-in-go


## Epilogue

Make connection to kraken. The next level is to improve resilience on the services (timeouts and Contexts).
