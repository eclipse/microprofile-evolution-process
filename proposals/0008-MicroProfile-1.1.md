# MicroProfile 1.1

* Proposal: [MP-0008](0008-MicroProfile-1.1.md)
* Authors: [Kevin Sutter](https://github.com/kwsutter), [John Clingan](https://github.com/jclingan)
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: 
[Pull Request Discussion](https://github.com/eclipse/microprofile-evolution-process/pull/30), 
[MicroProfile 1.1 Proposal Discussion](https://groups.google.com/forum/#!topic/microprofile/AFobwjU7z6E)

## Introduction

MicroProfile 1.1 is the first update to the successful MicroProfile 1.0 release at
JavaOne 2016.  At the recent Devoxx UK 2017 conference, the idea for a MicroProfile 1.1
release was announced and discussed.  The specific content for this MicroProfile 1.1
release is not finalized for this proposal.  The goal delivery date for MicroProfile 1.1
is 2Q2017.

Mailinglist thread: [MicroProfile Roadmap Discussion](https://groups.google.com/forum/#!topic/microprofile/zTAuSTe6_So)

## Motivation

We need to continue the momentum for the MicroProfile effort and community.  This next 
release will demonstrate that we are actively progressing the MicroProfile programming
model.  It will also demonstrate that we're extending beyond the limits of Java EE (since
MicroProfile 1.0 only contained Java EE specifications).

## Proposed solution

The proposed solution is to define MicroProfile 1.1 as building upon the current content of
MicroProfile 1.0 plus whatever sub-features are "ready" when the MicroProfile 1.1 release
is cut.  One leading contender for inclusion in MicroProfile 1.1 is the Config 1.0 API.
The Config API 1.0 release is nearing completion and is in the process of shutting down
this initial release.

Another possible inclusion is the Fault Tolerance 1.0 API, but it will depend on the 
readiness of this feature when MicroProfile 1.1 is cut off.  Depending the exact timing
of the MicroProfile 1.1 cutoff, there may be other sub-features considered for inclusion, 
such as JWT Propagation or Health Metrics. 

## Detailed design (if applicable)

N/A.  The detailed design, api, implementation, and test cases are contained within the
respective sub-features to be included.

## Impact on existing code (if applicable)

N/A.  MicroProfile 1.1 will contain the features from MicroProfile 1.0 (JAX-RS 2.0, CDI
1.2, and JSON-P 1.0) along with these new "ready" features (ie, Config API 1.0, or 
Fault Tolerance API 1.0, or ...).

## Alternatives considered

Covered above.
