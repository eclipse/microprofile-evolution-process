# MicroProfile 1.1

* Proposal: [MP-0008](0008-MicroProfile-1.1.md)
* Authors: [Kevin Sutter](https://github.com/kwsutter), [John Clingan](https://github.com/jclingan)
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: ...

## Introduction

MicroProfile 1.1 is the first update to the successful MicroProfile 1.0 release at
JavaOne 2016.  The proposed content for MicroProfile 1.1 is MicroProfile 1.0 plus 
[Config API](https://github.com/eclipse/microprofile-evolution-process/blob/master/proposals/0001-config.md) and
[Fault Tolerance API](https://github.com/eclipse/microprofile-evolution-process/blob/master/proposals/0004-FaultTolerance.md).  
The details of these individual features can be found in their respective
proposals and repositories.

Mailinglist thread: [MicroProfile Roadmap Discussion](https://groups.google.com/forum/#!topic/microprofile/zTAuSTe6_So)

## Motivation

We need to continue the momentum for the MicroProfile effort and community.  This next 
release will demonstrate that we are actively progressing the MicroProfile programming
model.  It will also demonstrate that we're extending beyond the limits of Java EE (since
MicroProfile 1.0 only contained Java EE specifications).

## Proposed solution

The proposed solution is to define MicroProfile 1.1 as containing both the Config 1.0 API and
the Fault Tolerance 1.0 API.  The Config API 1.0 release is nearing completion and is 
"ready" for inclusion.  The Fault Tolerance API 1.0 release needs to shut down to be 
"ready" for MicroProfile 1.1.  It doesn't have everything, but it's close enough to be
included in 1.1.  And, we can continue working a Fault Tolerance 1.1 API to fill in the 
gaps for a future MicroProfile 1.x.

I've heard the arguments that we should only include the features that are absolutely
ready for MicroProfile 1.1.  The problem with that approach is if MicroProfile 1.1 goes 
out the door with only the Config 1.0 API, we won't be taken seriously.  We need to show
more progress.  And, the Fault Tolerance 1.0 API will show that progress.  We've said 
from the beginning that MicroProfile was going to be an iterative process.  We can add
to the Fault Tolerance capabilities in a follow-on release.

## Detailed design (if applicable)

N/A.  The detailed design, api, implementation, and test cases are contained within the
respective sub-features:

* [Config API Repo](https://github.com/eclipse/microprofile-config)
* [Fault Tolerance API Repo](https://github.com/eclipse/microprofile-fault-tolerance)

## Impact on existing code (if applicable)

N/A.  MicroProfile 1.1 will contain the features from MicroProfile 1.0 (JAX-RS 2.0, CDI
1.2, and JSON-P 1.0) along with these new features (Config API 1.0, Fault Tolerance API 
1.0).

## Alternatives considered

Covered above.
