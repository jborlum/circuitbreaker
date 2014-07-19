# circuitbreaker

Circuitbreaker provides an easy way to use the Circuit Breaker pattern in a
Go program.

Circuit breakers are typically used when your program makes remote calls.
Remote calls can often hang for a while before they time out. If your
application makes a lot of these requests, many resources can be tied
up waiting for these time outs to occur. A circuit breaker wraps these
remote calls and will trip after a defined amount of failures or time outs
occur. When a circuit breaker is tripped any future calls will avoid making
the remote call and return an error to the client. In the meantime, the
circuit breaker will periodically allow some calls to be tried again and
will close the circuit if those are successful.

You can read more about this pattern and how it's used at:
- [Martin Fowler's bliki](http://martinfowler.com/bliki/CircuitBreaker.html)
- [The Netflix Tech Blog](http://techblog.netflix.com/2012/02/fault-tolerance-in-high-volume.html)
- [Release It!](http://pragprog.com/book/mnee/release-it)

## Installation

```
  go get github.com/rubyist/circuitbreaker
```

## Examples

Here is a quick example of what circuitbreaker provides

```go
// Creates a circuit breaker that will trip if the function fails 10 times
cb := NewCircuitBreaker(10)

cb.BreakerOpen = func(c *CircuitBreaker, err error) {
	// This function will be called every time the circuit breaker moves
	// from closed to open, passing in the circuit breaker object and
	// the error returned by the failing function
}

cb.BreakerClosed = func(c *CircuitBreaker) {
	// This function will be called every time the circuit breaker moves
	// from open to closed, passing in the circuit breaker object
}

cb.Call(func() error {
	// This is where you'll do some remote call
	// If it fails, return an error
})
```

Circuitbreaker can also wrap a timeout around the remote call.

```go
// Creates a circuit breaker that will trip after 10 failures or timeouts
// using a time out of 5 seconds
cb := NewTimeoutCircuitBreaker(10, 5)

// Proceed as above

```

## Bugs, Issues, Feedback

Right here on GitHub: [https://github.com/rubyist/circuitbreaker](https://github.com/rubyist/circuitbreaker)