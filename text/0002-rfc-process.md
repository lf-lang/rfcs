- Feature Name: `rfc_process`
- Start Date: 2024-02-05
- RFC PR: [lf-lang/rfcs#2](https://github.com/lf-lang/rfcs/pull/2)
- Tracking Issue(s): None

# Abstract
[abstract]: #abstract

Establish a Request for Comments (RFC) process. The RFC process is intended to
provide a consistent and controlled path for proposing new features to the
Lingua Franca language, the compiler infrastructure and tooling, as well as the
runtime libraries. The goal is to allow all stakeholders to participate in the
design of new features and to establish confidence in the direction the language
and tooling is involving in.

# Motivation
[motivation]: #motivation

Lingua Franca started as a research project and major contributions to the
language and infrastructure have been mostly driven by the research goals of
individual contributors. While this approach allowed for quickly building a rich
set of features, it also created significant heterogeneity within the project.
To further scale the development effort, to professionalize the code base, and
to establish Lingua Franca as a mature platform, a more uniform and disciplined
approach is required.

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

Text to be placed in the README of this repository:

---

[Lingua Franca RFCs]: #lf-rfcs

The "RFC" (request for comments) process is a framework for managing both
historical and planned changes within the LF community. The record of active
RFCs tells the story of our project, our technology and our thought process.
We believe in an honest dialog between all contributors and stakeholders within
the community.

Many changes, including bug fixes and documentation improvements can be
implemented and reviewed via the normal GitHub pull request workflow. However,
some changes are more substantial and we ask to put them through a bit of a
design process. In particular, changes that impact other contributors or
stakeholders in the LF community or changes that considerably change the user
experience. See [When you need to follow this process] for more details.

## Table of Contents
[Table of Contents]: #table-of-contents

  - [Opening](#lf-rfcs)
  - [Table of Contents]
  - [Goals and Non-Goals]
  - [The RFC Process]
  - [Implementation of active RFCs]
  - [When you need to follow this process]
  - [License]
  - [Contributing]


## Goals and Non-Goals
[Goals and Non-Goals]: #goals-and-non-goals

### Goals

- Create a platform for proposing ideas and features, receiving feedback and
  suggestions for improvement, and quickly iterating towards an improved design.
  Timely is more important than perfect.
- Establish a record of the relevant design considerations and decisions. This
  is an important piece of documentation as it allows us to find and reference
  past design considerations and is a valuable resource for newcomers in the team.
- Provide a process for producing a consensus among the contributors and
  stakeholders directly affected by the proposed change. For substantial changes
  to the semantics or the textual and graphical syntax of the language, the
  community as a whole should be involved.
- Make submitting RFCs enjoyable. Ideally the RFC process should be perceived as
  a supporting tool that helps submitters to asses the feasibility of an idea
  early on (before sinking time into an implementation) and by helping to
  improve the design.

### Non-Goals

- Slowing down development unnecessarily, creating significant overhead, or
  requiring wide consensus on every decision. The process should be flexible
  enough to adapt to the concrete scope and impact of a proposal and only
  require a wide consensus for changes that impact large parts of the community.
- Specification or even standardization of language features and interfaces. If
  we decide to create specifications for certain aspects of the language and the
  infrastructure, this is should be done in another process, as specification
  incurs a considerably larger overhead. Assuming that we establish a
  specification process, RFCs could still play a role in it, e.g., for early
  discussion of additions to the specification.
- Establishing a waterfall model for software design. While RFCs could be
  interpreted as a specification, they explicitly are not. On the contrary, RFCs
  are intended as a tool for iterating on ideas quickly and more frequently,
  even before there is an implementation.
- Discussing complete implementations. For this we have PRs.


## The RFC Process
[The RFC Process]: #the-rfc-process

In brief, we use GitHub pull requests to implement the RFC process. This
repository contains all accepted (active) RFCs. To propose a new feature, you
draft a design document following our [template](0000-template.md) and open a
pull request to ask for comments. The PR will be reviewed by the stakeholders
and other interested parties. You help to build a consensus and incorporate
concerns or suggestions in the document. At some point, we will decide to merge
the PR, in which case the RFC becomes active, or close the PR, in which case the
RFC is rejected. In either case, the PR discussion captures the rationale and
explanation of the decision.

#### Drafting an RFC

Creating a new RFC should be something you start as soon as it feels like your
work is substantial and worth getting additional opinions on. You can start an
RFC by forking this repository and copying the
[`0000-template.md`](0000-template.md) file to `text/0000-my-feature.md`,
replacing `my-feature` with a descriptive short name for your proposal.
Fill in the current date in the top section, but leave the RFC PR and tracking
issue unchanged for now. Fill in the sections of the template to start your
proposal.

Don't overthink the proposal or aim for being perfect. The idea of the RFC
process is to receive feedback quickly and iterate frequently. Indicate any open
questions that you are aware of.

#### Receiving early feedback and collaborative editing

Create a new branch (the branch name should be identical to the short name you
chose for the proposal file), commit your new proposal and push it to this
repository or to your own fork. Share the link to your new file with your peers
to invite others do collaborate on the proposal or ask for early feedback.
Ideally this involves one of the more senior contributors, who can help to avoid
larger pitfalls. If you are new to the community and don't know who to reach out
to, drop a message in our [Zulip](https://lf-lang.zulipchat.com/). The proposal
does not need to be perfect (timely is more important than perfect), but you
should clearly identify any gaps that you are aware of.

At this early stage using git and GitHub is merely a suggestion. You may also
choose other forms of exchanging ideas with your peers and collaborating on a
text document.

#### Opening a pull request

Once you have build trust in your proposal and are ready to receive feedback
from a wider audience, it is time to open a pull request on this repository.
Once you have opened the PR, replace `0000` in the file name of your RFC with
the PR number and insert a link to the PR at the top of your RFC.

Opening a PR will allow the stakeholders and interested community members to
review your proposal and to raise concerns or make suggestions. We will also
assign a [shepherd](#role-of-the-shepherd) to your PR who helps managing the
remaining process. At this stage, the shepherd should request reviews from
relevant stakeholders and community members that are affected by or generally
interested in the RFC.

#### Addressing feedback

Reviewers will share there valuable thoughts and opinions. Aim to address this
feedback and, if necessary, make changes to the RFC in order to help reach a
consensus. The shepherd may help to moderate the discussion if needed.

Make changes as new commits to the pull request, and leave a comment on the pull
request explaining your changes. Specifically, do not squash or rebase commits
after they are visible on the pull request.

Not every concern raised by a reviewer is an indication of a bad design and
requires a significant change to the RFC. No solution is perfect and usually
there are trade-offs involved. Thus addressing a concern can be as simple as
acknowledging it as a drawback in the RFC document. Again: timeliness and
frequent iteration are more important than being perfect.

If the RFC is discussed during a meeting, notes and a summary of the meeting
should be shared as a comment in the PR thread.

#### Final comments and decision

After the discussion settles down, the shepherd decides how to proceed. There
are three options:

1. If there is an obvious consensus in the discussion and the shepherd is
   confident that all relevant stakeholders noticed the RFC, the shepherd may
   decide to merge (accept) or close (reject) the PR directly.
2. If there is no obvious consensus or there is feedback from major stakeholders
   missing, the shepherd makes a final call for comments with a disposition to
   either accept or reject the RFC. This gives the reviewers the possibility to
   wrap up the discussion. The shepherd may set a deadline until which final
   comments should be made, but must ensure that this deadline can be met by the
   stakeholders. If the reviewers agree with the disposition, the PR gets merged
   in case of accept or closed in case of reject.
> **Open Question:** What exactly should be required to reach a decision?
> Majority? Unanimously? Not more than two objections?
3. For RFCs that require due diligence, as they propose considerable changes to
   the syntax and semantics of the language or changes to the tooling that
   largely impact how users interact with the language, the process in 2. gets
   extended slightly. If the reviewers agree on a disposition to accept, the
   shepherd announces a final comment period (FCP) on the mailing list and in
   the Zulip chat. This gives everyone in the community a chance to raise
   additional concerns. The final comment period lasts 10 days and, if no now
   concerns are brought, up the shepherd merges the PR once the 10 days have
   passed.

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
knows the major stakeholders. Typically, this will be a long-time contributor of
LF. During the RFC process the shepherd acts as a moderator and is responsible
for identifying and notifying the relevant stakeholders. While the shepherd may
weigh in on the discussion and on a decision as a reviewer, they are not a
decision maker. Their role is to moderate the decision making process and to
decide if more stakeholders or the entire community should be roped in to the
process.

## Implementation of active RFCs
[Implementation of active RFCs]: #implementation-of-actions-rfcs

Once an RFC becomes active, anyone may submit an implementation of the feature
as a pull request to the relevant repo. Being active, however, does not
guarantee that an implementation of the feature will ultimately be merged; it
does mean that in principle all the major stakeholders have agreed to the
feature and are amenable to merging it.

The nature of software development is that new insights are gained during
implementation and requirements change frequently. Thus, no design document can
perfectly reflect the end product. Active RFCs are not a specification and you
may deviate from the initial design where needed (this is even encouraged!).
Please document those deviations in the implementation PR. Active RFCs may also
be modified in a follow up pull-request to incorporate new insights. However, if
a more significant change of the design is required, a new RFC should be created
with a note added to the original RFC.

A core idea of the RFC process is to think about the implications of a new
feature and its design early on and to receive feedback quickly and frequently.
This helps to identify potential concerns and major roadblocks before investing
in a concrete implementation. However, it might be helpful to support arguments
for or against an RFC by a prototype implementation. For some high-priority
features it might also be required to work on the implementation alongside the
RFC. However, if you decide to invest in an implementation before the RFC
becomes active, the existence of this implementation and the work that you put
in do not present arguments against concerns raised by the RFC reviewers.

## When you need to follow this process
[When you need to follow this process]: #when-you-need-to-follow-this-process

You need to follow this process if you intend to make "substantial" changes to
Lingua Franca, lingo, the reactor runtimes, or the RFC process itself. What
constitutes a "substantial" change is evolving based on community norms and
varies depending on what part of the ecosystem you are proposing to change, but
may include the following.

  - Any semantic or syntactic change to the language that is not a bugfix.
  - Removing language features.
  - Changes to the interface between the compiler and the runtime libraries.
  - Changes to the interfaces between other components of the LF ecosystem, e.g., the language server.
  - New user-facing features in the IDEs and related tools.
  - Changes to the graphical syntax of Lingua Franca.
  - Changes that require significant updates of the handbook.

Some changes do not require an RFC:

  - Rephrasing, reorganizing, refactoring, or otherwise "changing shape that does
    not change meaning".
  - Additions that strictly improve objective, numerical quality criteria
    (warning removal, speedup, better platform coverage, more parallelism, etc.)
  - Additions only likely to be noticed by other developers and invisible to users of Lingua Franca.
  - Trivial changes that can be implemented and discussed in a single PR.

If you submit a pull request to implement a new feature without going through
the RFC process, it may be closed with a polite request to submit an RFC first.

## License
[License]: #license

The contents of this repository are licensed under the 2-Clause BSD License (see [LICENSE](LICENSE)).

## Contributing
[Contributing]: #contributing

Unless explicitly stated otherwise, any contribution intentionally submitted for inclusion in this repository shall be licensed as above, without any additional terms or conditions

---

Text to be placed in `0000_template.md`:

---

- Start Date: (fill me in with today's date, YYYY-MM-DD)
- RFC PR: [lf-lang/rfcs#0000](https://github.com/lf-lang/rfcs/pull/0000)
- Tracking Issue(s): [lf-lang/lingua-franca#0000](https://github.com/lf-lang/lingua-franca/issues/0000)

# Abstract
[abstract]: #abstract

A one paragraph explanation of the feature. This may even be a single sentence.

# Motivation
[motivation]: #motivation

Describe why your proposal is relevant, which use cases it supports, and what we expect the outcome to be.

# Proposed Implementation
[proposed-implementation]: #proposed-implementation

This is the core of your proposal, and its purpose is to help you think through the problem because writing is thinking.

- Clearly name and introduction new concepts.
- Use diagrams to help illustrate your ideas where applicable.
- Include LF code examples and use-cases if you propose changes to the language.
- Also consider to use code snippets and examples if you are proposing an interface or
  system contract.
- Describe the interaction with other components and features.
- Consider to include a guide-level explanation that outlines how you would
  explain this feature to a user (in case of user-facing changes), or to another
  contributor in case of internal changes. Ideally, this serves as a draft for
  later documentation of the feature.

# Drawbacks
[drawbacks]: #drawbacks

Describe possible reasons for not following up with your proposal and identity
the associated costs. This section will likely get expanded on during the review
process.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

- Which alternative designs where considered?
- Is there prior art and what can we learn from it?
- Why is the proposed design the best design?

# Unresolved questions
[unresolved-questions]: #unresolved-questions

Gather open questions that should be answered before the RFC gets accepted and
either list them in the document using markdown quoting or add them to this
section. If you want to mark open questions inline use the following format,
which is easy to spot when reading through the document:

```
> **Open Question**: this is an open question.
```

You may also list further unresolved questions that are beyond the scope of the
RFC process. This may include implementation specific questions that can be
resolved as part of the implementation process or questions that can be
addressed by future RFCs independent of the outcome of this RFC.

This section may be left blank if there are no obvious unresolved questions at
time of submission.

# Future possibilities
[future-possibilities]: #future-possibilities

This is a place to collect further ideas that are beyond the scope of the RFC
topic. This is an optional section and may be left blank. It can be used to
collect ideas that pop up during the review process.

---

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

The proposed RFC process is heavily inspired by the [Rust
RFCs](https://github.com/rust-lang/rfcs). The description above was adjusted
slightly to better fit our current community structure and scale.

When accepted, the above description will become the content of the main README
file in the repository and define the process we use for proposing, reviewing,
discussing and accepting/rejecting RFCs. We will further add the file called
`0000-template.md` to the root of the repository to act as a template for new
RFCs.

# Drawbacks
[drawbacks]: #drawbacks

- RFCs could be perceived as a chore rather than an enjoyable collaborative
  process.
- RFCs impose a more formal style of designing and discussion new features,
  which potentially could slow down development.
- RFCs could be interpreted as a strict specification and implemented in a
  water-fall style without considering changing requirements and new insights
  gained from the implementation.
- RFCs focus on the design, not an implementation. Thus, active RFCs will rarely
  describe the end result and may often need to be adjusted when the feature gets
  implemented.

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

A variation of the proposed RFC process has been used successfully by the Rust
community for the last 10 years. The process is intended to enforce a more
disciplined approach while being as light-weight as possible. The main rationale
for this proposal is to adapt an established and successful RFC process to the
LF community. This proposal is intended as a starting point. Likely, the
proposed process will not fit exactly our needs and can be modified in future
RFCs.

In order to be lightweight, the proposed process only considers the design of a
feature and does not require an implementation. This allows to focus on the
high-level implications of a proposal as part of the RFC process, while the
implementation details can be discussed in specific PRs in the code
repositories. When new insights are gained from an implementation, those should
be reflected in updates to the RFC.

Alternatively, we could incorporate the implementation process into the RFC
process. This would significantly prolong the process, but ensure that new
insights from the implementation are directly reflected in the RFC discussion.

There are many potential alternatives to the proposed RFC process. The [Python
enhancement proposals (PEPs)](https://peps.python.org/pep-0001/), for instance,
uses a more formal process and it is challenging for a new PEP to be accepted.
Since we are still a relatively young community a more lightweight process
appears to be appropriate.

Another alternative would be to continue as we did and not establish an RFC
process. This approach, however, will likely not scale to a larger community and
harden the problems we are facing with respect to an overarching design and
architecture.


# Prior art
[prior-art]: #prior-art

- The proposed RFC process is heavily inspired (this is not to say shamelessly copied) from the [Rust community](https://github.com/lf-lang/rfcs)
- Python uses [PEP (Python enhancement proposals)](https://peps.python.org/pep-0001/), which is similar to the idea of RFCs but slightly more formal.
- The IETF RFC process is explained [here](https://www.ietf.org/standards/process/informal/).
- Many companies use RFCs internally. For instance Uber has used RFCs sucessfully to scale from a small team to a thousand engineers as described [here](https://blog.pragmaticengineer.com/scaling-engineering-teams-via-writing-things-down-rfcs/).

# Unresolved questions
[unresolved-questions]: #unresolved-questions

- Which license should we use for RFCs in this repository?
- How can we send out FCPs to all interested parties? (The mailing list would
  work, but currently there is no way for people to subscribe)

# Future possibilities
[future-possibilities]: #future-possibilities

- The process can be partially automated using a bot. This is done by the Rust
community. However, for now, we should try to establish a process that works for
us (and refine it) before automating the process.

- We can automatically generate a website that lists all active RFCs. The Rust
community uses mdBook for this.

- The Rust community is split into sub-teams. Each RFC gets assigned to a
sub-team and is mostly discussed within this team, which may also involve
stakeholders outside of the team. When the LF community scales we may consider
using a similar approach and assign clearer responsibilities.

- The line between changes that can be done in a simple PR and more substantial
  changes that require an RFC is blurry. In fact, there might be changes where
  RFCs cause too much of an overhead while a PR does not provide enough room for
  up-front discussion. In this case, other processes that focus on specific
  aspects of the project, i.e. the architecture of the compiler, can be
  introduced and adjusted to the specific requirements of the sub-community.

- The proposed process and template are merely a starting point. It can (and
  should) be adjusted to our needs once we gain more experience.

- The RFC process does not consider how we can achieve a level of feature parity
  between the targets. This aspect could be considered in future RFCs.
