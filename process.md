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
responsible for the strategic direction of Microprofile. *Core team members
initiate, participate in, and manage the public review of proposals
and have the authority to accept or reject changes to Microprofile.*

In accordance with the requirements of Eclipse projects, you must have an [ECA on file](https://wiki.eclipse.org/Development_Resources/Contributing_via_Git#Eclipse_Contributor_Agreement) with the foundation
and all commits that you raise to any Eclipse Microprofile repository, whether direct or via pull requests, must follow their [guidelines](https://wiki.eclipse.org/Development_Resources/Contributing_via_Git#Signing_off_on_a_commit).  Please review their entire [contributing
guide](https://wiki.eclipse.org/Development_Resources/Contributing_via_Git) first.

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

The review process for a particular proposal begins with a pull request of a new or updated proposal into
the [Microprofile repository](https://github.com/microprofile/evolution). The  core team and the wider community will review the proposal and comment on it.

Reviews usually last one or two weeks, but can run longer for
particularly large or complex proposals.

When a review is scheduled, the core team will post
the proposal to the [Microprofile mailing
list](https://groups.google.com/forum/#!forum/microprofile) with the subject "[Review]"
followed by the proposal title and update the list of active
reviews. To avoid delays, it is important that the proposal authors be
available to answer questions, address feedback, and clarify their
intent during the review period.

After the review has completed, the core team will make a decision on
the proposal. The core team members will report their decision
to the proposal authors and mailing list. The proposal state
in the [Microprofile
repository](https://github.com/microprofile/evolution) will be updated to reflect that decision.

## Proposal states
A given proposal can be in one of several states:

* **Awaiting review**: The proposal is awaiting review. It should be added into the [list of proposals](https://github.com/eclipse/microprofile/tree/master/proposals). When the
  review period is announced (on the mailing list), the state transitions to
  *Active review*.
* **Active review**: The proposal is being reviewed publicly after a review period is announced. The review may happen either in a dedicated thread in the mailing list or within the pull request with the proposal. After the specified review period, the Core team (core committers) should vote about accepting the proposal ASAP.
* **Returned for revision**: The core team decided to return the proposal
  for additional revision to the current draft. The proposal transitions to the *Awaiting review* state.
* **Withdrawn**: The proposal has been withdrawn by the original submitter before accepted.
* **Deferred**: Consideration of the proposal has been deferred
  because it does not meet the [goals of the upcoming major Microprofile
  release](README.md). Deferred proposals will be reconsidered when
  scoping the next major Microprofile release. After reconsidered, the state may transition to the *Awaiting review* state.
* **Accepted**: The proposal has been accepted by the core team and the proposed specification is either awaiting
  implementation or is actively being implemented.
* **Rejected**: The proposal has been considered by the core team and is rejected. This is a final state. A proposal in the *Rejected* state can only be resubmitted as a new proposal. Compare with the *Deferred* state, which is not a final state.
* **Implemented (VERSION)**: The proposed specification has been implemented.
  Append the version number in parentheses—for example: Implemented (1.0), or more specificlly with the specfication name: MicroProfile Config (1.0).
* **Included (VERSION) in MicroProfile (MP-VERSION)**: An implemented specification, which is also included in a MicroProfile version. For example: MicroProfile Config 1.0 (Microprofile 1.1)

## Review announcement

When a proposal enters review, an email using the following template will be
sent to the Microprofile-announce and Microprofile mailing lists:

---

Hello Microprofile community,

The review of "PROPOSAL NAME" begins now and runs through REVIEW
END DATE. The proposal is available here:

> <https://github.com/microprofile/evolution/blob/master/proposals/NNNN-proposal.md>

Reviews are an important part of the Microprofile evolution process. All reviews
should be sent to the Microprofile mailing list at

> <https://groups.google.com/forum/#!forum/microprofile>


##### What goes into a review?

The goal of the review process is to improve the proposal under review
through constructive criticism and, eventually, determine the direction of
Microprofile. When writing your review, here are some questions you might want to answer in your review:

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

The MicroProfile team

---

## Steps after a proposal has been accepted

* Announce in the review mailing thread that the proposal has been accepted and that repositories are to be created
* Raise a bug for Eclipse to create a Github repsitory or ask for migration of an existing repository (see the [Eclipse Handbook](https://www.eclipse.org/projects/handbook/#resources-github))
** When repositories are created, announce in a new thread that the proposed ideas can be worked on in the new repository towards a specification.
