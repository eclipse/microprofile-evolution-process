# Fault Tolerance

* Proposal: [MP-0004](0004-FaultTolerance.md)
* Authors: [Emily Jiang](https://github.com/Emily-Jiang), [Jonathan Halterman](https://github.com/jhalterman/)
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: [Discussion thread topic covering the  Rationale](https://groups.google.com/forum/#!topic/microprofile/ezFC1TLGozU), [Discussion thread topic with additional Commentary](https://groups.google.com/forum/#!forum/microprofile)

## Introduction

It is increasingly important to build fault tolerant micro services. Fault tolerance is about leveraging different strategies to guide the execution and result of some logic. Retry policies, bulkheads, and circuit breakers are popular concepts in this area. They dictate whether and when executions should take place, and fallbacks offer an alternative result when an execution does not complete successfully. 

As mentioned above, the Fault Tolerance proposal is to focus the aspects: RetryPolicy, Fallback, bulkhead and circuit breaker.
* RetryPolicy: Define a criteria on when to retry 
* Fallback: provide an alternative solution for a failed execution.
* Bulkhead: isolate failures in part of the system while the rest part of the system can still function.
* CircuitBreaker: offer a way of fail fast by automatically failing execution to prevent the system overloading and indefinite wait or timeout by the clients.

The main design is to separate execution logic from execution. The execution can be configured with fault tolerance policies, such as RetryPolicy, Fallback, Bulkheader and CircuitBreaker. 

Hysterix(https://github.com/Netflix/Hystrix) and Failsafe(https://github.com/jhalterman/failsafe) are two popular libraries for handling failures. This proposal is to define a standard API and approach for applications to follow in order to achieve the fault tolerance.

The requirements are as follows:
* Loose coupling: Execution logic should not know anything about the execution status or fault tolerance. 
* Failure handling strategy should be configured when the execution takes place.
* Support for synchronous and asynchronous execution
* Integration with 3rd party asynchronous APIs. This is necessary to handle executions that are completed at some time in the future, where retries will need to be explicitly scheduled from within the asynchronous execution. This is common when working with various 3rd party asynchronous tools such as Netty, RxJava, Vert.x, etc.
* Support immutable failure handling policy configuration
* Some Failure policy configurations, e.g. CircuitBreaker, RetryPolicy, can be used stand alone. For example, it has been very useful for circuit breakers to be standalone constructs which can be plugged into and intentionally shared across multiple executions. Likewise for retry policies. Additionally, an Execution construct can be offered that allows retry policies to be applied to some logic in a standalone, manually controlled way.

Advanced requirements:
* Event Hooks: Since this approach to fault tolerance involves handing execution over to some foreign code, it's very useful to be able to learn when executions are taking place (before/after) and under what circunstances (onRetry, onFailedAttempt, etc). 


Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!topic/microprofile/ezFC1TLGozU)

## Motivation

Currently there are at least two libraries to provide fault tolerance. It is best to uniform the technologies and define a standard so that micro service applications can adopt and the implementation of fault tolerance can be provided by the containers if possible.


## Proposed solution
Separating the responsibility of executing logic (Runnables/Callables/etc) from guiding when execution should take place (through retry policies, bulkheads, circuit breakers). In this way, failure handling strategies merely become configuration that can influence executions, and the execution API itself is just responsible for receiving some configuration and performing executions.

By default, a failure handling strategy could assume, for example, that any exception is a failure. But in some cases, a null or negative return value could be considered a failure. Users should be able to define this, and a user's definition of a failure is what should influence execution. (This all is what the Failsafe RetryPolicy's retryOn, retryWhen, abortIf, etc methods are all about - defining a failure).

Standardise the Fallback, Bulkhead and CircuitBreaker APIs and provide implementations.
* A Java API to privide RetryPolicy
* A Java API to provide FallBack
* A Java API to provide BulkHead
* A Java API to provide circuitBreaker

## Detailed design


An instance of RetryPolicy, FallBack and BulkHeader can be injected to the clients and then they can be configured as follows.

* RetryPolicy: A policy define the retry criteria
RetryPolicy rp = retryPolicy.retryOn(TimeOutException.class)
  .withDelay(2, TimeUnit.SECONDS)
  .withMaxRetries(2);
When TimeOutException was received, delay 2 seconds and then retry 2 more times.
Connection connection = Execution.with(rp).withFallBack(null).get(this::connect)
The above suppress the TimOutException and provide a default result.

* CircuitBreaker: a rule to define when to close the circuit
CircuitBreaker cb = circuitBreaker.withFailureThreshold(3, 10)
  .withSuccessThreshold(5)
  .withDelay(1, TimeUnit.MINUTES);
Connection connect = Execution.with(cb).run(this::connect);  
When 3 of 10 execution failures occurs on a circuit breaker, the circuit is opened and further execution requests fail with CircuitBreakerOpenException. After a delay of 1 min, the circuit is half-opened and trail executions are attempted to determine whether the circuit should be closed or opened again. If the trial executions exceed a success threshold 5, the breaker is closed again and executions will proceed as normal.

* Bulkhead
BulkHead bh = bulkHead.withPool("myPool");
Connection connect = Execution.with(bh).run(this::connect);
Bulkhead provides a thread pool with a fixed number of threads in order to achieve thread and failure isolation.  
## Impact on existing code

n/a

## Alternatives considered

n/a