# Distributed tracing

* Proposal: [MP-0005](0005-DistributedTracing.md)
* Authors: [Akihiko Kuroda](https://github.com/akihikokuroda), [Steve Fontes](https://github.com/Steve-Fontes)
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

The following are the four items proposed for providing distributed tracing in microprofile.io:
1. Support configuration of an opentracing.io compliant Tracer.  
	(Specification does not need to contain how this would be implemented.)

2. Provide support in the framework to enable basic distributed tracing without modification to existing microprofile.io applications.  
	(Specification does not need to contain how this would be implemented.)

3. Provide support in the framework to add distributed tracing records explicitly where needed by the developer.  
  This would be implemented with four annotations:  
	@StartDistributedTrace(operation=&lt;tracepointname&gt;, tags=&lt;map of tags&gt;, log=&lt;map of log data&gt;, baggage=&lt;map of baggage&gt;)  
	@FinishDistributedTrace(operation=&lt;tracepointname&gt;, tags=&lt;map of tags&gt;, log=&lt;map of log data&gt;, baggage=&lt;map of baggage&gt;)  
	@DistributedTrace(operation=&lt;tracepointname&gt;, tags=&lt;map of tags&gt;, log=&lt;map of log data&gt;, baggage=&lt;map of baggage&gt;)    
	Applied to a block of code - Span started at beginning of block, Span finished at end of block.
	@AppendToDistributedTrace(operation=&lt;tracepointname&gt;, tags=&lt;map of tags&gt;, log=&lt;map of log data&gt;, baggage=&lt;map of baggage&gt;)  
	Adds tags, log data or baggage to the active span.  
	The "tags", "log", and "baggage" parameters are optional for all annotations.
  The operation names would have to match between StartDistributedTrace and FinishDistributedTrace operations.

4. Provide programmatic access for distributed tracing operations
	This would be implemented by providing access to the configured Tracer object.

	**How do we want to do that? I don't think we want GlobalTracer.get() - probably something more CDIish.**

	I think that is all we would need to make full access to opentracing.io function available.
	Since it appears by https://github.com/opentracing/opentracing-java/pull/115 that the opentracing.io specification for Java will include
	ActiveSpan span = tracer.activeSpan();
	and support around that, we don't need to define how to expose active span as part of microprofile.io, that part of the opentracing.io spec will just have to be implemented in microprofile.io.

## Detailed design
TBD

## Impact on existing code
Per item 2, we will be able to enable distributed tracing for applications with no change to application code. This will have no affect on the logic of the application, but will cause distributed trace records to be produced.

## Alternatives considered
Current mechanisms require a decision at development time about the distributed trace system that will be used.
This feature allows the decision to be made at the operational environment level.
