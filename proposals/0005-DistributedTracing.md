# Distributed tracing

* Proposal: [MP-0005](0005-DistributedTracing.md)
* Authors: [Akihiko Kuroda](https://github.com/<yourname>), [Steve Fontes](https://github.com/Steve-Fontes)
* Status: **Awaiting review**
* Decision Notes: [Discussion thread topic covering the  Rationale](https://groups.google.com/forum/#!topic/microprofile/YxKba36lye4)

## Introduction

Distributed tracing allows you to trace the flow of a request across service boundaries.
This is particularly important in a microservices environment where a request typically flows through multiple services.
To accomplish distributed tracing, each service must be instrumented to log messages with a correlation id that may have been propagated from an upstream service.
A common companion to distributed trace logging is a service where the distributed trace records can be stored. ([Examples](http://opentracing.io/documentation/pages/supported-tracers.html)).
The storage service for distributed trace records can provide features to view the cross service trace records associated with particular request flows.

It will be useful for services written in the microprofile.io framework to be able to integrate well with a distributed trace system that is part of the larger microservices environment.

Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!topic/microprofile/YxKba36lye4)

## Motivation

In order for a distributed tracing system to be effective and usable, two things are required
1. The different services in the environment must agree on the mechanism for transferring correlation ids across services.
2. The different services in the environment should produce their trace records in format that is consumable by the storage service for distributed trace records.

Without the first, some services will not be included in the trace records associated with a request.
Without the second, custom code would need to be written to present the information about a full request flow.

There are existing distributed tracing systems that provide a server for distributed trace record storage and viewing, and application libraries for instrumenting microservices.
The problem is that the different distributed tracing systems use implementation specific mechanisms for propagating correlation IDs and for formatting trace records,
so once a microservice chooses a distributed tracing implementation library to use for its instrumentation, all other microservices in the environment are locked into the same choice.

The [opentracing.io project's](http://opentracing.io/) purpose is to provide a standard API for instrumenting microservices for distributed tracing.
If every microservice is instrumented for distributed tracing using the opentracing.io API, then (as long as an implementation library exists for the microservice's language),
the microservice can be configured at deploy time to use a common system implementation to perform the log record formatting and cross service correlation id propagation.
The common implementation ensures that correlation ids are propagated in a way that is understandable to all services,
and log records are formatted in a way that is understandable to the server for distributed trace record storage.

In order to make microprofile.io distributed tracing friendly, it will be useful to allow distributed tracing to be enabled on any microprofile.io application,
without having to explicitly add distributed tracing code to the application.

In order to make microprofile.io as flexible as possible for adding distributed trace log records, microprofile.io should expose whatever objects are necessary for an application to use the opentracing.io API.

## Proposed solution

The [opentracing.io](http://opentracing.io) API provides a mechanism to include distributed tracing instrumentation across services written in different languages.
The following are the three items proposed for providing distributed tracing in microprofile.io:
1. Support opentracing.io as the underlying API for distributed tracing.
2. Provide support in the framework to enable basic distributed tracing without modification to existing microprofile.io applications.
3. Provide support in the framework to access opentracing.io objects for explicit distributed trace instrumentation within microprofile.io applications.

### For item 1 - opentracing.io as the API for distributed tracing:  
I don't think there is any implementation decisions here that would go across microprofile.io implementations. Just have to agree that applications written to the opentracing.io API will be supported.

### For item 2 - Support distributed tracing without application modification
This proposal does not need to explicitly define how item 2 will be implemented, since the implementation does not need to be exposed to the developer writing a microprofile.io application.
However, the implementation must support the API's defined for item 3.
We will need to at least provide a NOOP Tracer implementation. Behavior of the NOOP tracer should be agreed on.

### For item 3 - Support opentracing.io API for explicit distributed trace instrumentation
Questions we have to answer for this item:
1. How do we provide access to the Tracer object?
2. How do we provide access to the Span that was created when a request arrived?
3. What objects will the NOOP Tracer return?
4. (Possibly) How do we provide access to the Span that is most recently active?

#### Existing opentracing.io API implementation for jax-rs
There is currently an implemetation of the opentracing.io API for jax-rs at [opentracing-contrib/java-jaxrs/](https://github.com/opentracing-contrib/java-jaxrs/).

This implementation passes the Tracer as an argument to the constructor of various objects, and then accesses the Tracer as a variable within the object.

This implementation stores the Span created when the request arrives as a property of the ContainerRequestContext. The arrival Span can be retrieved whenever the ContainerRequestContext is available. The arrival Span needs to be made available in an application specific way if you want to use it as a parent Span when making outbound requests.

With this implementation, maintaining the most recently active span (if needed) is the responsibility of the application.

A second implementation of the opentracing.io API for jax-rs is at https://github.com/uber/jaeger-client-java.

#### Proposed microprofile.io API for opentracing.io
##### Access to Tracer object
TBD
##### Access to upstream Span
TBD
##### Access to current active Span
TBD
##### Behavior of NOOP Tracer
TBD

## Detailed design
TBD

## Impact on existing code
Per item 2, we will be able to enable distributed tracing for applications with no change to application code. This will have no affect on the logic of the application, but will cause distributed trace records to be produced.


## Alternatives considered
Current mechanisms require a decision at development time about the distributed trace system that will be used.
This feature allows the decision to be made at the operational environment level.
