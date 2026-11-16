---
rfc: 0001
title: RFC Process
process: facilitated
status: implemented(2026-11-02)
---

## Motivation

The Dart team currently does not have a single process for making project-wide
technical decisions. This means individual contributors and teams must seek
consensus in a decentralized fashion, without leaving a clear trace of how and
why certain decisions were made. The RFC process proposed here aims to close
this decision gap, make it easy to make project-wide technical decisions and
once a decision is made leave behind an engineering artifact which captures how
and why such decision was made.

The RFC process described here is also completely open to external contributors.
You do not have to be part of the Dart or Flutter teams to initiate it.

## Stakeholders

**Stakeholders:** Dart and Flutter TLs

**Socialization:** Dart steering has reviewed the proposal before it was shared
with TLs.

## Design

This section describes the RFC process.

### When to use the process

The vast majority of changes to Dart do not require an RFC. Instead, these
changes can be made using the code review process. However, technical decisions
that have broad impact across the project require broader agreement and must be
socialized with the project using the RFC process.

The RFC process provides a standardized framework for communicating design
proposals, collecting and if necessary incorporating feedback on designs, and
documenting the discussion leading to the acceptance or rejection of a design.
Facilitation by the Dart Technical Council (DTC) can help to identify and engage
the relevant set of stakeholders while keeping discussion focused on the
important aspects of the proposal.

Some RFCs exist simply to document and ratify team-wide decisions. But the vast
majority of RFCs are expected to be *design docs*. When such an RFC is accepted,
it means that the project decided to solve a problem in a certain way for
specific reasons.

There are two variants of the RFC process: **facilitated**, with
facilitation by the DTC, and **self-driven** without such facilitation.

The following kinds of changes must use the **facilitated** RFC process:

- **Adding constraints on future development.** Some decisions, once made,
  constrain the future development of the system. We need to be careful when
  making such decisions because they can be difficult to revise later. Examples
  of changes that fall into this category: adding a new public API or ABI to the
  SDK, committing to supporting a new target execution platform, adding new
  language features.

- **Making project policy.** Project policies have broad impact across the
  system, often affecting contributors throughout the project.

- **Changing Dart SDK architecture.** The architecture describes how the SDK
  fits together as a whole. Changing its architecture, by definition, crosses
  boundaries between subsystems and requires careful consultation with many
  stakeholders. Examples of changes that fall into: introducing a new compiler
  into the SDK, unifying two or more different Dart front-ends, adding a plugin
  system to a tool shipped in the SDK, adding a new tool to the SDK, adding a
  new SDK variant, committing to a stable ABI for specific artifacts produced by
  SDK tools.

- **Breaking Changes.** Changes which fall under
  [Dart SDK breaking change process](https://github.com/dart-lang/sdk/blob/main/docs/process/breaking-changes.md)
  (see also [Breaking changes process](#breaking-changes-process) below).

- **New third-party SDK dependencies.** There are strict requirements for
  packages that Dart SDK depends on. Adding a package that does not satisfy
  these requirements requires approval via the RFC process.

- **Delegating decision-making authority.** There are classes of decisions
  that the project needs to make frequently and that benefit from specialized
  expertise. Rather than making all these decisions through the RFC process, the
  project can delegate decision-making authority for those classes of decisions
  to another group or process. For example, as part of proposing the RFC process
  itself we also propose special extensions to it for changing the programming
  language and the core libraries.

- **Escalations**. Finally, contentious changes can benefit from the
  transparency and clarity of the RFC process. If there is a disagreement about
  technical direction that cannot be resolved by an individual technical leader,
  the decision can be escalated to the RFC process either by one of the
  disagreeing parties or by another contributor.

The RFC process may also be used for any other changes that would benefit from
its structured approach to decision making and its durable record of the
decision. The RFC process is also a way to establish durable relationships
between a specific workstream and appropriate stakeholders and possibly shift
future decisions downwards - allowing faster decision making in the future.

### Roles and responsibilities

People interact with the RFC process in several roles:

- **RFC Authors**. An RFC Author is a person who writes an RFC. Anybody can be
  an RFC Author. A given RFC can have one or more authors. The authors of a
  given RFC drive the process for that RFC.

- **Stakeholder**. A stakeholder is a person who has a stake in whether the
  project accepts a given RFC. Stakeholders are typically Dart contributors, but
  some RFCs might have stakeholders beyond these projects. For example,
  stakeholders might be involved in other projects that use Dart or are
  otherwise affected by changes to Dart. Stakeholders do not always participate
  directly in discussions about RFCs. Instead, stakeholders are often
  represented by someone, often a technical lead or other person responsible for
  a group of stakeholders.

- **Facilitator**. A representative of the Dart Technical Council (DTC)
  appointed to the RFC - if the **facilitated** RFC process is used.
  Facilitator is responsible for helping authors with the RFC process. Their
  responsibilities include: helping authors identify correct stakeholders,
  handling escalations during discussions, deciding if the RFC is ready to enter
  the [Final Comment Period (FCP)](#step-3-final-comment-period-fcp), and
  communicating the DTC decision to approve or reject the RFC.

- **Dart Technical Council**. The DTC makes the final decision as to whether the
  project accepts an RFC.

### How the process works

This section describes each step involved in the RFC process.

```
                                                          +----------+    +-------------+
                                                     +--> | ACCEPTED | -> | IMPLEMENTED |
+-------+          +---------+          +-------+    |    +----------+    +-------------+
| DRAFT | -------> | DISCUSS | -------> |  FCP  | ---+
+-------+          +---------+          +-------+    |    +----------+
    |                   | ^                |         +--> | REJECTED |
    |                   | |                |              +----------+
    |  +-----------+    | |                |
    +->| WITHDRAWN |<---+ +--- NEEDS WORK -+
       +-----------+
```

#### Socialize

Before you kick off the formal RFC process, socialize your idea with the
project. For example, you might have noticed a problem that you think is
important to solve. Are other people aware of this problem? Someone else might
already be working on the problem or might have some background or context about
the problem that would be useful to you. The earlier you discover this
information, the better.

Compared to the remaining steps in the process, this step is relatively
informal. This document does not contain a rigorous description of how to
socialize your idea. Socializing technical ideas is a skill unto itself.
However, a good place to start is to raise the topic in discussions with the
technical leads for areas related to the problem you are trying to solve. For
example, you might want to consult with people in the OWNERS files for the areas
of the codebase that will need to be modified to execute your idea.

If you are unsure how to socialize your idea, consider asking a technical leader
for advice. They will often have more experience socializing ideas and might be
able to point you in a good direction.

#### Step 1: Draft

Once you have gathered all the background and context you can through
socialization, you are ready to start the formal part of the RFC process. The
next step is to write a first draft of the RFC document itself.

Mechanically, an RFC is a markdown file in the
[https://dart.googlesource.com/rfc/](https://dart.googlesource.com/rfc/)
([https://github.com/dart-lang/rfc](https://github.com/dart-lang/rfc))
repository. To create an RFC, you create a Gerrit CL that adds a file to that
repository. You should start by making a copy of
[the RFC template](../0000_TEMPLATE.md). While not strictly required, the template
is designed to guide you towards writing a high-quality RFC by prompting you
to think through the problem you are trying to solve in a semi-structured way.

Do not worry about assigning a number to your RFC at this stage. Instead, use
`0000` as a placeholder. For example, the file name should be something like
`rfcs/0000_my_idea/0000_my_idea.md`. The RFC will get a number once it is
approved or rejected, before landing.

After uploading the first version of the RFC CL you will need to decide which
variant of the RFC process you are going to use: **facilitated** or
**self-driven**.

For a **facilitated** RFC add technical-council-public@dartlang.org to the CC
list of the CL. This will notify the DTC and a member of the DTC will be
appointed as a dedicated *facilitator*, who serves as your point of contact with
the DTC. The DTC might also decide that the proposal does not need to be
facilitated and suggest to switch to **self-driven** variant.

A **self-driven** RFC will not have a dedicated facilitator assigned.

Being the first version of the RFC does not imply it being incomplete or raw.
You can enter the process with a fully fleshed out design. The most common
reason to take this path would be that your team operates its own established
design process (see [Language evolution process](#language-evolution-process)
as an example). In that case simply author an RFC capturing the outcome of your
design process, making it clear how design work was done, who participated as a
stakeholder, etc. The facilitator might suggest entering the FCP stage directly
if they deem that all appropriate stakeholders have been involved and that
discussions have settled.

In general we expect that RFCs will be predominantly public in nature and as
such can be discussed entirely in the publicly accessible space - Dart's public
[code review tool](https://dart-review.googlesource.com/q/project:rfc).
However, for rare instances where an RFC can't be discussed in public -
the internal [code review instance](https://dart-internal-review.googlesource.com/q/project:rfc)
will be used instead, keeping the rest of the process the same.
Private RFCs will be moved into the public RFC repository once privacy is no
longer required.

**Note**: The Dart project requires RFCs to be authored and owned by humans.
RFCs based primarily on LLM-generated content -- even if lightly edited -- will
be rejected and sent back for a rewrite. Authors must be able to personally
explain and defend all proposed designs. Repeated violations may result in a ban
from contributing to the Dart project as a whole.

#### Step 2: Discuss

Once you have created a CL containing the first draft of your RFC, you are ready
to iterate on your idea with the appropriate stakeholders. If the RFC is
**facilitated** then facilitator appointed by the DTC will ensure that all
relevant stakeholders are identified and invited to provide the feedback.
Hopefully you will have already discovered at least some of them as part of
socializing your idea. If the RFC is **self-driven** then authors and
stakeholders are responsible for identifying all relevant stakeholders.

Mechanically, you and the facilitator will invite stakeholders to provide
feedback on the RFC by adding them to the "Reviewers" or "CC" fields in the CL,
as you would for a normal code review. The default RFC template also encourages
you to list stakeholders and their focus areas in the RFC itself. The
stakeholders should provide you with feedback by leaving comments on your RFC in
the code review tool.

If the discussion is too complex for the code review tool, you can split more
complex discussion threads into separate issues at
[https://github.com/dart-lang/rfc](https://github.com/dart-lang/rfc) repository.
These issues must be tagged with `rfc-cl<clnumber>` label and referenced back
from the Gerrit CL using an unresolved comment. Once discussion on a specific
issue converges - the corresponding issue should be closed and the Gerrit thread
resolved with a comment summarizing the achieved resolution.

If needed consider scheduling a meeting with the relevant stakeholders to have a
more efficient discussion. After the meeting, consider if posting a summary of
the meeting on the CL (as a comment) would benefit stakeholders who were not
present in the meeting.

If the discussion becomes contentious, escalate to the facilitator (if one is
assigned) or to the DTC directly (for self-driven RFCs). The Technical Council
can help move the discussion forward, for example by providing additional
structure to the discussion or moving the discussion to another forum.
Regardless of how the discussion proceeds, the results of any off-CL discussion
must be captured in the CL, often by posting a summary of the discussion as a CL
comment.

If you wish to withdraw your RFC, you can mark the CL containing the RFC as
abandoned. You, or someone else, can always resurrect your RFC later if
circumstances change. If you are resurrecting an RFC created by someone else,
you should start the RFC process over from the beginning, but you can use the
withdrawn RFC as a starting point rather than the template. Please confer
with the original authors to determine whether they wish to continue to have
their names associated with the new incarnation of the RFC.

##### Moderation

Everybody involved in the discussion should engage in the RFC discussion
respectfully and according to Dart's
[Code of Conduct](https://dart.dev/community/code-of-conduct). Any violations
should be escalated to the DTC (either via facilitator or directly). The Council
can warn and ban participants violating the Code of Conduct or otherwise
derailing the discussion. Gerrit has builtin tools for dealing with abuse and
spam.

#### Step 3: Final Comment Period (FCP)

At some point the iterations on the RFC will converge: that is you will reach a
point where you believe that you have involved all appropriate stakeholders and
you have incorporated or addressed relevant stakeholder feedback. At this point
RFC should be ready to move to the final comment period (FCP) stage, in which
stakeholders make the final decision on whether they support the RFC or object
to it.

If the RFC is facilitated then you should notify the facilitator about your
intent to enter FCP. The facilitator will then review the RFC and the current
state of the discussion. If they don't see anything wrong they will move it to
the FCP for the minimum of 7 days. Otherwise, the facilitator will explain what
is missing e.g. highlight specific comment threads where stakeholders raised
issues which were not addressed, missing stakeholders, or similar shortcomings.
The facilitator's focus at this stage will be on the *process*, not the content
of the proposal.

If the RFC is self-driven then you should simply post a comment on the CL that
RFC is entering FCP.

During the FCP stakeholders who wish to support the RFC sign-off on it by voting
`Sign-Off +1`. Stakeholders who wish to object vote `Sign-Off -1` and provide
their reason for objecting, in a way that would let anyone understand the
objection clearly without having to read the entire discussion that preceded the
objection. The clarity of the objection is important as the DTC will make the
decision based on the text of the RFC and rationales associated with
stakeholder's votes - without necessarily reading through all discussion threads
on the RFC.

A stakeholder's objection (`Sign-Off -1`) does not necessarily prevent the DTC
from accepting the RFC. See the
[How the DTC makes decisions](#how-the-dtc-makes-decisions) section below for
more details about how the project decides whether to accept an RFC.

If stakeholders wish more time to consider the RFC they can in exceptional cases
request to extend the FCP period.

If the RFC is facilitated then at the end of the FCP but before the DTC
starts reviewing the votes the RFC authors should amend the RFC text with a
separate section rebutting individual objections.

#### Step 4: Decision (for facilitated process)

At the end of the final comment period the DTC will review stakeholder's votes
and make the decision. There are three possible outcomes.

- **DTC accepts the RFC.** The facilitator will comment on your CL stating that
  the RFC is accepted and assigning a number to your RFC. For any objection
  (`Sign-Off -1`) votes, the DTC decision is expected to describe why the RFC is
  moving forward despite the objections.
- **DTC rejects the RFC.** The facilitator will comment on your CL stating that
  the RFC is rejected, assigning the RFC a number and providing a rationale for
  the rejection. Rejected RFCs are valuable engineering artifacts.
- **DTC sends the RFC back to the discussion**, because it needs more work. In
  this case, the facilitator is expected to provide the rationale for this
  decision and changes the DTC expects to see at the end of the discussion. In
  general, we expect this to be a rare outcome - the DTC will exercise their
  best judgement to reserve this outcome for cases where there is a clear reason
  to believe that proposal should be approved but more discussion is required to
  address substantive objections.

If the DTC has accepted or rejected the RFC, update your RFC with the assigned
number, both in the title of the RFC and in the filename. If your RFC is
rejected, you will need to include rejection rationale into the RFC as a
separate section.

Once the RFC CL is updated the facilitator will vote `Code-Review +1` allowing
you to land the RFC.

Congratulations! You have contributed a valuable engineering artifact to the
project!

#### Step 4: Decision (for self-driven process)

At the end of the final comment period there are three possible outcomes:

- There are no objection (`Sign-Off -1`) votes and there is at least one
  supporting (`Sign-Off +1`) vote on the RFC. In this case **RFC is accepted.**
- There are no supporting (`Sign-Off +1`) votes and at least one objecting
  (`Sign-Off -1`) vote. In this case **RFC is rejected.**
- If nobody voted or votes are mixed then **RFC is send back to discussion.**

For accepted and rejected RFCs the author has to request an RFC number from the
DTC before landing. If your RFC is rejected, you will need to include rejection
rationale into the RFC as a separate section.

If the author or stakeholders are not satisfied with the outcome they can choose
to escalate to the DTC.

#### Step 5: Implementation and In-Progress Revisions

If your RFC is accepted and requires implementation, you should update its
status when implementation starts and later when it completes. You can choose to
add references to issues tracking implementation work and later to release blog
posts or public documentation.

For some RFCs an implementation attempt might reveal unanticipated issues or
gaps in the original design which require revisions of the RFC. In such
occasions RFC authors should prepare a change to the already landed RFC and send
it for review. All original stakeholders and the DTC facilitator should be CCed
on the change.

Small revisions which do not significantly change the spirit of the proposal can
be reviewed and approved by anybody with the relevant context. Large revisions
would need to go through the same Discuss and FCP steps as the full RFC.

In very rare cases, a situation can arise where implementation feedback
completely invalidates the whole premise of the original RFC - in such
situations the author can choose to return RFC to the Discuss stage or Withdraw
it entirely. In both cases RFC authors should land CL which will update RFC text
to clearly mark its changed status. RFCs withdrawn due to implementation
feedback are useful engineering artifacts and should stay in the repo similar to
rejected RFCs.

In general, accepted RFCs are perceived as mostly immutable. As such
stakeholders are expected to adopt _"disagree and commit"_ philosophy when
reviewing subsequent revisions to an accepted RFC. Their comments are expected
to be focused on parts being revised by the author and they should refrain from
requesting additional modifications in unrelated sections unless absolutely
vital for coherency. An RFC author can consider unrelated requests for
modification as frivolous and escalate those to the DTC either via the
facilitator (for facilitated RFCs) or directly (for self-driven RFCs).

#### Later revisions

Once the RFC is implemented it is marked as such and considered immutable from
that point on.

RFCs can only be updated in rare circumstances:

- Minor edits to make RFCs easier to read and understand. Fixing grammatical or
  orthographical errors, adding links.
- If another RFC is implemented that impacts this RFC, e.g. makes it obsolete,
  augments or adjusts behaviors specified in the RFC - then a link to newer RFC
  can be added with an appropriate note to the original RFC.

All changes to implemented facilitated RFCs require review by a member of
the DTC. Implemented self-driven RFCs can be updated by author with a review by
one of the original stakeholders.

### How the DTC makes decisions

The decision whether to accept an RFC is made by the DTC, acting in
[rough consensus](https://datatracker.ietf.org/doc/html/rfc7282) with each
other. How exactly the DTC makes the decision is left intentionally
underspecified - however the DTC is expected to clearly communicate their
decision and the rationale back to the RFC authors in written form (see
[Step 4](#step-4-decision-for-facilitated-process) above).

If the DTC cannot reach a rough consensus, the RFC is not accepted. In deciding
whether to accept an RFC, the DTC will consider the following factors:

- Does the RFC advance the goals of the project?
- Does the RFC uphold the values of the project?
- Were all of the stakeholders appropriately represented in the discussion?
- If any stakeholders objected, does the DTC understand the objections fully?

Decisions made by the DTC can be escalated to the governing authority for the
Dart project - which currently is Dart's engineering director.

### Changes to the existing processes

The Dart team has several processes which are similar to the RFC process in
either intent or nature. These processes will be adjusted with the introduction
of the RFC process.

#### Language evolution process

Dart has an existing process for making changes to the language (see
[Lifecycle of a language feature](https://github.com/dart-lang/language/blob/main/doc/life_of_a_language_feature.md#lifecycle-of-a-language-feature)).
This process is already very similar to the RFC process:

- A feature specification is a design artifact similar to an RFC - it describes
  the problem a particular language feature is trying to address and captures
  the proposed syntax and semantics of a new language feature.
- Dart's language team authors feature specifications, involving appropriate
  stakeholders across Dart and Flutter teams to gather feedback on different
  aspects (developer experience, impact on implementation, performance
  characteristics, etc).
- Design work on feature specifications happens in a dedicated GitHub repo
  dart-lang/language using a combination of PRs and discussion issues.
  - Initial version of the proposal is landed into the repo and then iterated
    in-place using a combination of PRs and fanned-out issues tracking different
    threads of discussion.
- Feature specifications transition through different stages (*working*,
  *accepted*, *implemented*).

We will make small targeted adjustments to further align the language evolution
process with the RFC process, without requiring the language team to
significantly change their workflow. In short, the language evolution process
will operate as an nested instance of the RFC process, with the language team
taking the role of the DTC.

Specifically this means the following changes:

- **Facilitation.** When appropriate (e.g. when a change is driven by someone
  not on the language team), a member of the language team is assigned to
  facilitate any language proposal. Facilitator's responsibilities include:
  - helping to keep discussions on track,
  - serving as point of contact for escalations associated with proposal,
  - deciding when the proposal is ready to enter FCP,
  - communicating back the approve/reject/move-work-needed decision back to the
    author.
- **Discussion.** Discussion will continue to happen using dart-lang/language
  repo using established PR+issues approach. Feature specification templates
  will be updated to make it clear:
  - Where in the process each feature is
  - Who was consulted as a stakeholder
  - What is the current status of the discussion, is this feature ready to enter
    FCP or not?
- **FCP.** Language proposals can enter an FCP - a clear way to transition from
  *working* to *accepted* state. The language team is the one deciding whether
  or not a specific proposal is accepted.
- **RFC.** Once the language feature is accepted the language team can file an
  intent-to-implement-and-ship RFC following the normal RFC process.

#### Flutter RFC Process

See [Flutter RFC 000.0002 - Flutter RFC Review & Decision Process](https://github.com/flutter/rfc/blob/main/rfc/000.0002-flutter-rfc-review-process.md)

#### Breaking changes process

The breaking change process is documented
[here](https://github.com/dart-lang/sdk/blob/main/docs/process/breaking-changes.md).
Making a breaking change in the language requires:

1. Creating an issue on GitHub which describes the reason for the breaking
   change, expected impact and mitigation.
1. Notifying users, collecting feedback
1. Collecting approvals from *a set of Dart SDK approvers*.
1. Executing the change if approved.

The whole process is kicked off by an engineer proposing a breaking change and
is facilitated by a TPgM.

There are clear similarities to the RFC process: GitHub issue is the body of the
RFC, Dart SDK approvers and external Dart developers are stakeholders, TPgM
fills the role of the DTC facilitator advancing the proposal through stages.

This process in general functions rather well, with minor issues: lack of
automation or pre-defined timeline (FCP) means that breaking change requests can
languish in unapproved state for a while.

With the introduction of the RFC process we will change breaking changes process
to use the RFC process, with adjustments:

- RFCs corresponding to breaking changes are discussed and approved on GitHub,
- The DTC takes over facilitating them just like any other RFC
- The automation will handle state transitions (e.g. entering and leaving FCP)
- Once approved or rejected breaking change is automatically converted to an RFC
  text which can be submitted to the RFC repository.

#### Core libraries changes process

When making non-breaking and non-public API changes to core libraries an
engineer is expected to collect CR+1 votes from engineers representing all
implementations of Dart (e.g. native, dart2js, ddc, dart2wasm) as well as
engineers owning the API surface of core libraries. This is enforced via a
[Core-Library-Review submit requirement](https://github.com/dart-lang/sdk/blob/main/docs/Gerrit-Submit-Requirements.md#core-library-review).
Developers can (based on their own sound judgement) opt-out of the requirement
by adding escape-hatch footer to their CLs.

This process works very well balancing engineering velocity and surfacing more
impactful changes to relevant stakeholders. Thus it does not make sense to
change it.

A developer making a very complex change to core libraries can however choose to
submit an RFC for it and collect approvals that way instead.

#### Cherry-pick process

Dart has a
[process](https://github.com/dart-lang/sdk/blob/main/docs/Cherry-picks-to-a-release-channel.md)
for cherry picking changes into release channels. This process is fairly
lightweight (e.g. no significant documentation is required), but the approval
flow shares the similarity with proposed RFC process: there is a dedicated group
([mdb/dart-admin](http://mdb/dart-admin)) which is responsible for approving or
denying cherry-picks based on the feedback from stakeholders.

### Documentation

This RFC serves as documentation for the RFC process.

### Drawbacks, Alternatives, and Unknowns

The primary cost of implementing this proposal is that introducing a formal
decision-making process might slow down the pace of decision-making. The process
might be heavier than necessary for some kinds of decisions.

Recording decisions in the source repository has the effect of making those
decisions more difficult to change. That effect might be positive in some
scenarios, but the effect might also be negative in other scenarios.

The criteria in the "when to use the process" section attempts to mitigate this
drawback by scoping the process to consequential situations but such scoping is
bound to have false positives and false negatives.

There are a large number of possible alternative strategies for solving the
underlying problem. For example, we could use a decision-making process that
centers around a synchronous meeting, but such a process will have difficulty
scaling to a global open-source project. We could also have selected a different
decision-making mechanism that balanced more towards consensus or more towards
authority.

### Choice of Gerrit vs GitHub PR vs Google Docs

We have chosen Markdown-in-repo over Google Docs to make end-result artifacts
more discoverable.

We have chosen Gerrit (code review tool used by the Dart SDK) over
GitHub PRs (or Google Docs) because review experience for larger changes
with many threads of discussion is better in Gerrit.

We acknowledge that external contributors might not be as familiar with Gerrit
workflows as with GitHub PRs or Google Docs - but the overhead of learning this
workflows is nothing compared to the work required to produce a well rounded
RFC.

## Prior art and references

The core of this process as well as large parts of the text in this proposal are
lifted (sometimes verbatim) from Fuchsia's RFC process, which itself is based on
an amalgamation of IETF RFC, Rust RFC and Blink's Intent-to-implement processes.
