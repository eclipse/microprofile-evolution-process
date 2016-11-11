# Circuit Break & Retry Patterns

* Proposal: [MP-0002](0002-circuit-breaker.md)
* Authors: [John Ament](https://github.com/JohnAment)
* Review Manager: TBD
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: [Discussion thread topic covering the  Rationale](https://groups.google.com/forum/#!topic/microprofile/ezFC1TLGozU)

## Introduction

Circuit Breaker as a design pattern is not new, but is starting to see more adoption as more of our application interactions are distributed across many nodes.  While REST is a dominant integration point, we cannot rule out that some applications do not leverage REST and others provide a well built client API that encapsulates a lot of the work.

Within this proposal, I introduce an integration strategy inspecific way to perform a circuit breaker integration.  This decouples the communication channel from the bean method invocation.

Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!topic/microprofile/ezFC1TLGozU)

## Motivation

Adding circuit breakers at the platform level when dealing with microservices feels like a given, it seems like a common part of the SDK that simply should be there.  Adding it into Microprofile allows us to innovate that SDK and solicit input.

## Proposed solution

It would be specified that each implementation provides a CDI Interceptor for `@CheckCircuits` that allows a bean method to be demarcated as circuit breaker aware.  Failures of that method (throwing `Exception`) would cause the method to fail.  Depending on the state of the circuit, this may cause a fall back to be executed.  If the circuit is already seen as open, the method wouldn't be invoked and the fall back always invoked.

The actual pattern for the circuit can be modeled one of two ways:
- A platform inspecific definition by providing a `@Named` definition of the circuit
- Adding the `@Retries` annotation to a class or method and defining the appropriate strategy within.

It should be assumed that `@Retries` and the definition are synonymous, and one should work with the other.  I would also expect that we define ways to match the name strategy (I'm presently using method name, but may want to fall back to class name as well, as the typical client class will mirror a service it calls).

## Detailed design

See initial prototype at [cdi-circuit-break](https://github.com/johnament/cdi-circuit-breaker)

The proposal uses an interceptor to do the actual work.  Other impls would be available to do other strategies.  I would even be happy to see this move into some common code for all implementations to leverage instead of it needing to be re-baked each time.

## Impact on existing code

No functional impact on existing code by adding this capability.  It would be a wholy new feature.

## Alternatives considered

The Java EE proposed solution is REST specific.  It assumes that every client application is using JAX-RS client libraries.  
