# Microprofile Evolution Process

Microprofile is an open forum to optimize Enterprise Java for a microservices architecture by innovating across multiple implementations and collaborating on common areas of interest with a goal of standardization.

Microprofile is growing and evolving, guided by a community-driven process referred to as the Microprofile evolution process. This document outlines the Microprofile evolution process and how a feature grows from a rough idea into something that can improve the microservice development experience for millions of programmers.

## Scope

The Microprofile evolution process covers all changes to the Microprofile, including new profile extensions and APIs (no matter how small), changes to existing features or APIs, removal of existing features, and so on. Smaller changes, such as bug fixes, optimizations, or diagnostic improvements can be contributed via the normal contribution process; see [Contributing to Microprofile](https://microprofile.io/community/#contributing).

## Goals

The Microprofile evolution process aims to leverage the collective ideas, insights, and experience of the Microprofile community to improve Enterprise Java for a microservices architecture. Its two primary goals are:

* Engage the wider Microprofile community in the ongoing evolution of Microprofile, and
* Maintain the vision and conceptual coherence of Microprofile.

There is a natural tension between these two goals. Open evolution processes are, by nature, chaotic. Yet, maintaining a coherent vision requires some level of coordination. The Microprofile evolution process aims to strike a balance that best serves the Microprofile community as a whole.

## Participation

Everyone is welcome to propose, discuss, and review ideas to improve
the Microprofile on the [
mailing list]((https://groups.google.com/forum/#!forum/microprofile)). Before posting a review,
please see the section "What goes into a review?" below.

The Microprofile [core team](https://microprofile.io/community/#core-team) is
responsible for the strategic direction of Microprofile. Core team members
initiate, participate in, and manage the public review of proposals
and have the authority to accept or reject changes to Microprofile.

## What goes into a review?

The goal of the review process is to improve the proposal under review
through constructive criticism and, eventually, determine the
direction of Microprofile. When writing your review, here are some questions
you might want to answer in your review:

* What is your evaluation of the proposal?
* Is the problem being addressed significant enough to warrant a change to Microprofile?
* Does this proposal fit well with the feel and direction of Microprofile?
* If you have used other libraries with a similar feature, how do you feel that this proposal compares to those?
* How much effort did you put into your review? A glance, a quick reading, or an in-depth study?

Please state explicitly whether you believe that the proposal should be accepted into Microprofile.

## How to propose a change

* **Check prior proposals**: many ideas come up frequently, and may either be in active discussion on the mailing list, or may have been discussed already.  Please check the mailing list archives and this list for context before proposing something new.

* **Socialize the idea**: propose a rough sketch of the idea on the [mailing list](https://groups.google.com/forum/#!forum/microprofile), the problems it solves, what the solution looks like, etc., to gauge interest from the community.
* **Develop the proposal**: expand the rough sketch into a complete proposal, using the [proposal template](0000-template.md), and continue to refine the proposal on the evolution mailing list. Prototyping an implementation and its uses along with the proposal is encouraged, because it helps ensure both technical feasibility of the proposal as well as validating that the proposal solves the problems it is meant to solve.
* **Request a review**: initiate a pull request to the [Microprofile repository](https://github.com/microprofile/evolution) to indicate to the core team that you would like the proposal to be reviewed. When the proposal is sufficiently detailed and clear, and addresses feedback from earlier discussions of the idea, the pull request will be accepted. The proposal will be assigned a proposal number as well as a core team member to manage the review.
* **Address feedback**: in general, and especially [during the review period](https://microprofile.io/current-reviews), be responsive to questions and feedback about the proposal.

## Review process

The review process for a particular proposal begins when a member of
the core team accepts a pull request of a new or updated proposal into
the [Microprofile repository](https://github.com/microprofile/evolution). That core team
member becomes the *review manager* for the proposal. The proposal
is assigned a proposal number (if it is a new proposal), then enters
the review queue.

The review manager will work with the proposal authors to schedule the
review. Reviews usually last a single week, but can run longer for
particularly large or complex proposals.

When the scheduled review period arrives, the review manager will post
the proposal to the [Microprofile mailing
list](https://groups.google.com/forum/#!forum/microprofile) with the subject "[Review]"
followed by the proposal title and update the list of active
reviews. To avoid delays, it is important that the proposal authors be
available to answer questions, address feedback, and clarify their
intent during the review period.

After the review has completed, the core team will make a decision on
the proposal. The review manager is responsible for determining
consensus among the core team members, then reporting their decision
to the proposal authors and mailing list. The review manager will
update the proposal's state in the [Microprofile
repository](https://github.com/microprofile/evolution) to reflect that decision.

## Proposal states
A given proposal can be in one of several states:

* **Awaiting review**: The proposal is awaiting review. Once known,
  the dates for the actual review will be placed in the proposal
  document and updated in the [list of proposals](proposals.xml). When the
  review period begins, the review manager will update the state to
  *Active review*.
* **Scheduled for review (MONTH DAY...MONTH DAY)**: The public review of the proposal
  on the [Microprofile mailing list](https://groups.google.com/forum/#!forum/microprofile)
  has been scheduled for the specified date range.
* **Active review (MONTH DAY...MONTH DAY)**: The proposal is undergoing public review
  on the [Microprofile mailing list](https://groups.google.com/forum/#!forum/microprofile).
  The review will continue through the specified date range.
* **Returned for revision**: The proposal has been returned from review
  for additional revision to the current draft.
* **Withdrawn**: The proposal has been withdrawn by the original submitter.
* **Deferred**: Consideration of the proposal has been deferred
  because it does not meet the [goals of the upcoming major Microprofile
  release](README.md). Deferred proposals will be reconsidered when
  scoping the next major Microprofile release.
* **Accepted**: The proposal has been accepted and is either awaiting
  implementation or is actively being implemented.
* **Accepted with revisions**: The proposal has been accepted,
  contingent upon the inclusion of one or more revisions.
* **Rejected**: The proposal has been considered and rejected.
* **Implemented (Microprofile VERSION)**: The proposal has been implemented.
  Append the version number in parentheses—for example: Implemented (Microprofile 2.2).
  If the proposal's implementation spans multiple version numbers,
  write the version number for which the implementation will be complete.

## Review announcement

When a proposal enters review, an email using the following template will be
sent to the Microprofile-announce and Microprofile mailing lists:

---

Hello Microprofile community,

The review of "\<\<PROPOSAL NAME>>" begins now and runs through \<\<REVIEW
END DATE>>. The proposal is available here:

> <https://github.com/microprofile/evolution/blob/master/proposals/NNNN-proposal.md>

Reviews are an important part of the Microprofile evolution process. All reviews
should be sent to the Microprofile mailing list at

> <https://groups.google.com/forum/#!forum/microprofile>

or, if you would like to keep your feedback private, directly to the
review manager. When replying, please try to keep the proposal link at
the top of the message:

> Proposal link:
>>  http://linkToProposal

>  Reply text

>>  Other replies

##### What goes into a review?

The goal of the review process is to improve the proposal under review
through constructive criticism and, eventually, determine the direction of
Microprofile. When writing your review, here are some questions you might want to
answer in your review:

* What is your evaluation of the proposal?
* Is the problem being addressed significant enough to warrant a
  change to Microprofile?
* Does this proposal fit well with the feel and direction of Microprofile?
* If you have used other languages or libraries with a similar
  feature, how do you feel that this proposal compares to those?
* How much effort did you put into your review? A glance, a quick
  reading, or an in-depth study?

More information about the Microprofile evolution process is available at

> <https://github.com/microprofile/evolution/blob/master/process.md>

Thank you,

-\<\<REVIEW MANAGER NAME>>

Review Manager

---
