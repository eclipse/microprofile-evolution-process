# Service Healthchecks

* Proposal: [MP-0003](0003-health-checks.md)
* Authors: [Clement Escoffier](https://github.com/cescoffier), [Heiko Rupp](https://github.com/pilhuhn), [Heiko Braun] (https://github.com/heiko-braun)

* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: [Discussion thread topic covering the  Rationale](https://groups.google.com/forum/#!topic/microprofile/GDhgOguDIXw)

## Introduction

This document describes a protocol (wireformat, semantics and possible forms of interactions) between system components that need to determine the “liveliness” of computing nodes in a bigger architecture.


Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!topic/microprofile/jIZAKiu76ys)

## Motivation

Health checks are used to probe the state of a computing node from another other machine (i.e. kubernetes service controller) with the primary target being cloud infrastructure environments where automated processes maintain the state of computing nodes. In this scenario health checks are used to determine if a computing node needs to be discarded (terminated, shutdown) and eventually replaced by another (healthy) instance.

It’s not intended (although could be used) as a monitoring solution for human operators.

## Proposed solution

The proposed solution breaks down into two parts:

- A health checks protocol and wireformat description
- A Java API to implement health check procedures

## Detailed design

### Protocol

For a detailed description of the protocol see the companion document that the describes [the protocol and wireformat](0003-spec.md)

### Java API

The main API to describe health check results (responses) is the `HealthStatus` interface:

```
File path = new File(System.getProperty("user.home"));
long freeBytes = path.getFreeSpace();
long threshold = 1024 * 1024 * 100; // 100mb
return freeBytes>threshold ?
        HealthStatus.
                named("diskspace")
                .up()
                .withAttribute("freebytes", freeBytes) :
        HealthStatus.
                named("diskspace")
                .down()
                .withAttribute("freebytes", freeBytes);
```

### JAX-RS Integration

A possible integration with JAX-RS endpoints:
 ```
@Path("/app")
public class HealthCheckResource {

    @GET
    @Path("/diskspace")
    @Health
    public HealthStatus checkDiskspace() {
        [...]
    }

    @GET
    @Path("/something-else")
    @Health
    public HealthStatus checkSomethingElse() {
        [...]
    }
}
 ```

### CDI Integration

A possible integration with CDI:
 ```
@ApplicationScoped()
public class HealthChecks {

    @Produces    
    @Health
    public HealthStatus checkDiskspace() {
        [...]
    }

    @Produces    
    @Health
    public HealthStatus checkSomethingElse() {
        [...]
    }
}
 ```

## Alternatives considered

At the time  of this writing no alternative proposal have been known to us.
