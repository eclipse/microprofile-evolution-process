# Feature name

* Proposal: [MP-0001](0001-config.md)
* Authors: [Author 1](https://github.com/Emily-Jiang), [Author 2](https://github.com/<yourname>)
* Review Manager: TBD
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: [Discussion thread topic covering the  Rationale](https://groups.google.com/forum/#!topic/microprofile/JRJXHqXpHZA), [Discussion thread topic with additional Commentary](https://groups.google.com/forum/#!forum/microprofile)

## Introduction

Sometimes it is not realistic to package application configurations within the application. For example, some configurations need to be secured and/or dynamic or be configured based on the running environment. As a consequence, these configurations need to be stored externally. 

A number of open source projects such as Archaius, Apache Commons, Tamaya, DeltaSpike etc are 
trying to deal with dynamic and external configurations. Since no Configuration standard exists in JavaEE specifications, it makes the interop very difficult. 

The lack of Configuration standard significantly impacts the interoperability of microservices. Configuration is a central part of the microservice programming model. The Configuration needs to be standardised. This proposal is to address the requirements: standardise Configuration APIs and Reference Implementations. With the standardisation, all Java applications not only microservices should benefit from this effort.



Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!topic/microprofile/JRJXHqXpHZA)

## Motivation

Standardise Configuration API so the microservices can run in different environments seemlessly. The proposed Configuration should solve the follow three issues:
1. 

## Proposed solution

Standardise the Configuration APIs and implementations

## Detailed design

Not at this stage.

## Impact on existing code

n/a

## Alternatives considered

n/a