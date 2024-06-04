# Lingua Franca RFCs
[Lingua Franca RFCs]: #lf-rfcs

The RFC (request for comments) process is a framework for managing both
historical and planned changes within the LF community. The record of active
RFCs tells the story of our project, our technology and our thought process. We
value an honest and open dialog between all contributors and stakeholders within
the community. The RFC process establishes accountability, helps to identify and
navigate disparate goals, and builds confidence in the direction the language
and the tooling are evolving in.

This process is intended to help anyone interested in designing and/or
implementing a feature that they would like to see become part of the Lingua
Franca toolchain. While it is not required to write an RFC before submitting a
PR, it can be very helpful in validating and refining a proposal. Particularly
for user-facing features and anything that makes a semantic or syntactic change
to the language, this is very important. If a PR does not have an accompanying
RFC, the maintainers may therefore still require one be submitted in order to
vet the proposal.

Many changes, however, including bug fixes and documentation improvements, can
be implemented and reviewed via the normal GitHub pull request workflow. 

Examples of changes that would _not_ require an RFC:

  - Rephrasing, reorganizing, refactoring, or otherwise "changing shape that does
    not change meaning".
  - Additions that strictly improve objective, numerical quality criteria
    (warning removal, speedup, better platform coverage, more parallelism, etc.)
  - Additions only likely to be noticed by other developers and invisible to users of Lingua Franca.
  - Trivial changes that can be implemented and discussed in a single PR.


> [!TIP] If you unsure whether it advisable for you to write an RFC, feel
> welcome to ask the
> [maintainers](https://github.com/orgs/lf-lang/teams/maintainers) for advice.

> [!NOTE]
> **TL;DR:** Follow these steps to open a new RFC:
> 1. Fork or clone this repository.
> 2. Copy [`0000-template.md`](0000-template.md)to `rfcs/0000-my-feature.md`, replacing `my-feature` with a descriptive short name for your proposal.
> 3. Fill in the current date in the top section, but leave the RFC PR and tracking issue unchanged for now.
> 4. Fill in the sections of the template to start your proposal and push your changes to a branch with the same name as your proposal.
> 5. Share with your peers, ask for feedback, and update your proposal. Use our [Zulip](https://lf-lang.zulipchat.com/) if you are new to the community.
> 6. Open a Pull Request in this repository.
> 7. Insert the PR link in the top section of the proposal and replace `0000` in the file name with the PR number.
> 8. Wait for reviews, carefully read the comments, and try to incorporate feedback in your proposal.
>
> Detailed explanations of the steps can be found [here](#the-rfc-process).

## Goals
[Goals]:
#goals

- Create a platform for proposing ideas and features, receiving feedback, and
  quickly iterating towards an improved design. Timely is more important than
  perfect.
- Establish a record of the relevant design considerations and decisions. This
  is an important piece of documentation, as it allows us to find and reference
  past design considerations and is a valuable resource for new contributors.
- Provide a process that allows all contributors and stakeholders directly
  affected by the proposed change to comment and participate in the decision
  process. For substantial changes to the semantics or the textual and graphical
  syntax of the language, the community as a whole should be involved.
- Make submitting RFCs enjoyable. Ideally, the RFC process should be perceived as
  a supporting tool that helps submitters to assess the feasibility of an idea
  early on (before sinking time into an implementation) and by helping to
  improve the design.

## Non-Goals
[Non-Goals]:
#non-goals

- Slowing down development unnecessarily, creating significant overhead, or
  requiring wide consensus on every decision. The process should be flexible
  enough to adapt to the concrete scope and impact of a proposal, and only
  involve the stakeholders directly impacted by the community.
- Specification or even standardization of language features and interfaces. If
  we decide to create specifications for certain aspects of the language and the
  infrastructure, this should be done in another process, as specification
  incurs a considerably larger overhead. Assuming that we establish a
  specification process, RFCs could still play a role in it, e.g., for early
  discussion of additions to the specification.
- Establishing a waterfall model for software design. While RFCs could be
  interpreted as specifications, they explicitly are not. On the contrary, RFCs
  are intended as a tool for iterating on ideas quickly and more frequently,
  even before there is an implementation.
- Discussing complete implementations. For this, we have PRs.

## The RFC Process
[The RFC Process]: #the-rfc-process

In brief, we use GitHub pull requests to implement the RFC process. This
repository contains all accepted (active) RFCs. To propose a new feature, you
draft a design document following our [template](0000-template.md) and open a
pull request to ask for comments. The PR will be reviewed by the stakeholders
and other interested parties. You help to address concerns and incorporate
suggestions in the document. At some point, we will decide to merge the PR, in
which case the RFC becomes active, or close the PR, in which case the RFC is
rejected. In either case, the PR discussion captures the rationale and
explanation of the decision.

#### Drafting an RFC

Creating a new RFC should be something you start as soon as it feels like your
work is substantial and worth getting additional opinions on. You can start an
RFC by forking this repository and copying the
[`0000-template.md`](0000-template.md) file to `rfcs/0000-my-feature.md`,
replacing `my-feature` with a descriptive short name for your proposal.
Fill in the current date in the top section, but leave the RFC PR and tracking
issue unchanged for now. Fill in the sections of the template to start your
proposal.

Don't overthink the proposal or aim for perfection. The idea of the RFC
process is to receive feedback quickly and iterate frequently. Indicate any open
questions that you are aware of.

#### Receiving early feedback and collaborative editing

Create a new branch (the branch name should be identical to the short name you
chose for the proposal file), commit your new proposal, and push it to this
repository or to your own fork. Share the link to your new file with your peers
to invite others to collaborate on the proposal or ask for early feedback.
Ideally, this involves one of the more senior contributors, who can help to avoid
larger pitfalls. If you are new to the community and don't know whom to reach out
to, drop a message in our [Zulip](https://lf-lang.zulipchat.com/). The proposal
does not need to be perfect (timely is more important than perfect), but you
should clearly identify any gaps that you are aware of.

At this early stage, using git and GitHub is merely a suggestion. You may also
choose other forms of exchanging ideas with your peers and collaborating on a
text document.

#### Opening a pull request

Once you have built trust in your proposal and are ready to receive feedback
from a wider audience, it is time to open a pull request on this repository.
Once you have opened the PR, replace `0000` in the file name of your RFC with
the PR number and insert a link to the PR at the top of your RFC.

Opening a PR will allow the stakeholders and interested community members to
review your proposal and raise concerns or make suggestions. We will also
assign a [shepherd](#role-of-the-shepherd) to your PR who helps manage the
remaining process. At this stage, the shepherd should request reviews from
relevant stakeholders and community members that are affected by or generally
interested in the RFC.

#### Addressing feedback

The reviewers will share their valuable thoughts and opinions and evaluate the
RFC based on the [acceptance
criteria]()https://github.com/lf-lang/.github/blob/main/CONTRIBUTING.md#acceptance-criteria.
Aim to address this feedback and, if necessary, make changes to the RFC in order
to help reach an agreement. The shepherd may help to moderate the discussion if
needed. The acceptance criteria provide a guideline on

Make changes as new commits to the pull request, and leave a comment on the pull
request explaining your changes. Specifically, do not squash or rebase commits
after they are visible on the pull request.

Not every concern raised by a reviewer is an indication of a bad design and
requires a significant change to the RFC. No solution is perfect, and usually,
there are trade-offs involved. Thus, addressing a concern can be as simple as
acknowledging it as a drawback in the RFC document. Again, timeliness and
frequent iteration are more important than being perfect.

If the RFC is discussed during a meeting, notes and a summary of the meeting
should be shared as a comment in the PR thread.

#### Final comments and decision

After the discussion settles down, the shepherd sends out a final call for
comments (FCP) with a disposition to accept or reject the RFC. The FCP lasts 7
days and is announced both on the community mailing list and the [`#rfc` Zulip
channel](https://lf-lang.zulipchat.com/#narrow/stream/425418-RFCs). The
announcement should include the RFC title and abstract as well as a link to the
PR. At the next developer meeting after the FCP period ends, the maintainers
will discuss the RFC and reach a decision.

#### Accepted RFCs

If the RFC is accepted, the shepherd announces the new active RFC on the
community mailing list and in the [`#rfc` Zulip channel](https://lf-lang.zulipchat.com/#narrow/stream/425418-RFCs).

Furthermore, the shepherd creates an issue (or issues) that tracks the
implementation of the RFC in the appropriate repository (or repositories) and
inserts links at the top section of the RFC document. The shepherd may also
delegate this task to the RFC author or another person that would be responsible
for implementing the RFC.

#### The role of the shepherd
[The role of the shepherd]: #role-of-the-shepherd

The shepherd is a trusted individual who understands the community well and
knows the major stakeholders. Typically, this will be a
[maintainer](https://github.com/orgs/lf-lang/teams/maintainers). During the RFC
process, the shepherd acts as a moderator and is responsible for identifying and
notifying the relevant stakeholders. The main task of the shepherd is to steer
the discussions towards a conclusion. The shepherd also decides when to start
the FCP period to bring the RFC to the attention of the maintainers who make the
final decision.

## Implementation of active RFCs
[Implementation of active RFCs]: #implementation-of-actions-rfcs

Once an RFC becomes active, anyone may submit an implementation of the feature
as a pull request to the relevant repo. Being active, however, does not
guarantee that an implementation of the feature will ultimately be merged; it
does mean that, in principle, all the major stakeholders have agreed to the
feature and are amenable to merging it.

The nature of software development is that new insights are gained during
implementation and requirements change frequently. Thus, no design document can
perfectly reflect the end product. Active RFCs are not a specification and you
may deviate from the initial design where needed (this is even encouraged!).
Please document those deviations in the implementation PR. Active RFCs may also
be modified in a follow-up pull-request to incorporate new insights. However, if
a more significant change to the design is required, a new RFC should be created
with a note added to the original RFC.

A core idea of the RFC process is to think about the implications of a new
feature and its design early on, and to receive feedback quickly and frequently.
This helps to identify potential concerns and major roadblocks before investing
in a concrete implementation. However, it might be helpful to support arguments
for or against an RFC with a prototype implementation. For some high-priority
features, it might also be required to work on the implementation alongside the
RFC. However, if you decide to invest in an implementation before the RFC
becomes active, the existence of this implementation and the work that you put
in do not present arguments against concerns raised by the RFC reviewers.

## License
[License]: #license

The contents of this repository are licensed under the 2-Clause BSD License (see [LICENSE](LICENSE)).

## Contributing
[Contributing]: #contributing

Unless explicitly stated otherwise, any contribution intentionally submitted for inclusion in this repository shall be licensed as above, without any additional terms or conditions.

Please also consider the [contributing guidelines](https://github.com/lf-lang/.github/blob/main/CONTRIBUTING.md) of the lf-lang organization.
