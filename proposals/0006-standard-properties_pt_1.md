# Feature name

* Proposal: [MP-0006](0006-standard-properties_pt_1.md)
* Authors: [John D. Ament](https://github.com/johnament)
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: [Discussion thread](https://groups.google.com/forum/#!topic/microprofile/e58HZ5cNLGQ)

## Introduction

In order to build portable applications and frameworks across different Microprofile runtimes, certain assumptions need to be made about the runtime.  This includes other paired runtimes in addition the availability of certain configuration information.  This spec aims to define some basic properties that may be useful to users as well as framework developers to better integrate with Microprofile.

Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!topic/microprofile/e58HZ5cNLGQ)

## Motivation

While we've defined basic server runtime capabilities (CDI + JAX-RS + JSON-P) we have not defined simple things such as that a service is avilable over HTTP.  It is implied with the notion of Microservices.

## Proposed solution

The proposed solution includes some basic properties that define your server runtime, which may include things like:

- `microprofile.listen.address` - the hostname/IP of where your service listens
- `microprofile.listen.port` - the port that your service is listening on

These properties aren't meant to be exhaustive, but instead represent some basic configuration of your system runtime that others may find useful.  As the spec is actually defined, we would anticipate listing out an exhaustive list for a to be determined Microprofile release.

Using the configuration API, a developer may be able to read their listen port or address using the following code snippet:

```
String listenAddress = ConfigProvider.getConfig().getValue("microprofile.listen.address", String.class);
int listenPort = ConfigProvider.getConfig().getValue("microprofile.listen.port", Integer.class);
```

Note that these vales would be read only from the user's point of view, there would no need to modify them, meaning placing these properties in a `ConfigSource` would not need to be supported.

## Detailed design (if applicable)

N/A

## Impact on existing code (if applicable)

Vendors may choose to deprecate their equivalent vendor-specific properties that provide the same information, or leave them and provide these properties as view into the configuration.

## Alternatives considered

None considered.