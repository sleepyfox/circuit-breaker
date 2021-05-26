# Code Kata: Circuit-Breaker

A code kata based upon implementing the circuit breaker pattern.

Consider two services, Shop and Account. Shop is used by Customer, and Shop uses the Account service to look up customer details e.g. shipping address. In the event that Account is unavailable, shop can still checkout as guest, and processes can later reconcile sales to customer accounts.

Say that Account is slow, and sometimes times-out, because it has to work with an external Active Directory instance (or some such). Of course, we can implement retry logic, but if Account is running slow, it is likely that a cascade of retrys will take it down. One common way to stop this happening is to implement a circuit-breaker.

## The circuit breaker pattern

Between Shop and Account we interpose a proxy, our circuit breaker. Under normal circumstances the proxy just forwards requests to the Account - the ciruit is closed. What we want to happen is for the proxy to recognise when Account is down, and to stop forwarding requests to it for a period of time, that is to open the circuit, to allow the Account service to recover,

We can then close the circuit again after the recovery timeout.

In a more advanced implementation, we can limit the number of new requests that get forwarded to Account in order to make sure that it is indeed behaving normally, before resuming normal service. This state is sometimes called 'half-open'. Of course, a real circuit breaker cannot be half-open, it is either open or closed, but every metaphor is a double-edged sword.

## The kata, part one

Although this is normally implemented with microservices, we want to keep the kata simple. Implement two objects, a Shop and an Account. Shop sends a message to Account called getCustomerDetails. The getCustomerDetails message may return a customerAccount object, or an error.

The Account will be proxied by a third object, a circuit breaker. The circuit breaker will open if any request to Account returns an error. It will stay open for ten seconds, and then close again. Implement the system as described.

## The kata, part two

In part one we implemented a very simple circuit breaker. For part two let's implement a breaking condition that is more lifelike: the circuit is broken if more than two requests fail in any ten-second window.

## The kata, part three

Now let's implement the half-open part of the pattern. When the circuit breaker is open, after three seconds allow a single request through. If this request succeeds then close the circuit, else leave it open for another three seconds.

## License

This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) 
https://creativecommons.org/licenses/by-nc-sa/4.0/
