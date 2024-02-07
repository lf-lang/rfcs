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

The "RFC" (request for comments) process is intended to provide a consistent
and controlled path for changes to Lingua Franca (such as new features) so that all
stakeholders can be confident about the direction of the project.

Many changes, including bug fixes and documentation improvements can be
implemented and reviewed via the normal GitHub pull request workflow. Some
changes though are "substantial", and we ask that these be put through a bit of
a design process and produce a consensus among the Lingua Franca community.

The RFCs are not intended as a complete specification. Active RFCs are a form of
documentation that describe the design of features that are considered feasible
and relevant by the community. As requirements change and rarely a design
proposal can reflect the end result precisely, RFCs should be amended or
replaced as needed during the implementation process. At any time, the list of
active RFCs presents a snapshot of the communities view on the state of the
project and its potential next steps.

## Table of Contents
[Table of Contents]: #table-of-contents

  - [Opening](#lf-rfcs)
  - [Table of Contents]
  - [When you need to follow this process]
  - [Before creating an RFC]
  - [What the process is]
  - [The RFC life-cycle]
  - [Reviewing RFCs]
  - [Implementing an RFC]
  - [RFC Postponement]
  - [License]
  - [Contributions]


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
  - New user facing features in the IDEs and related tools.
  - Changes to the graphical syntax of Lingua Franca.
  - Changes thas require signifcant updates of the handbook.

Some changes do not require an RFC:

  - Rephrasing, reorganizing, refactoring, or otherwise "changing shape does
    not change meaning".
  - Additions that strictly improve objective, numerical quality criteria
    (warning removal, speedup, better platform coverage, more parallelism, etc.)
  - Additions only likely to be noticed by other developers and invisible to users of Lingua Franca.
  - Trivial changes that can be implemented and discusssed in a single PR.

If you submit a pull request to implement a new feature without going through
the RFC process, it may be closed with a polite request to submit an RFC first.

## Before creating an RFC
[Before creating an RFC]: #before-creating-an-rfc

A hastily-proposed RFC can hurt its chances of acceptance. Low quality
proposals, proposals for previously-rejected features, or those that don't fit
into the near-term roadmap, may be quickly rejected, which can be demotivating
for the unprepared contributor. Laying some groundwork ahead of the RFC can
make the process smoother.

Although there is no single way to prepare for submitting an RFC, it is
generally a good idea to pursue feedback from other project developers
beforehand, to ascertain that the RFC may be desirable; having a consistent
impact on the project requires concerted effort toward consensus-building.
You may talk the idea over on our [official Zulip server](https://lf-lang.zulipchat.com/)
or file issues on this repo for discussion.

As a rule of thumb, receiving encouraging feedback from long-standing project
developers is a good indication that the RFC is worth pursuing.

## What the process is
[What the process is]: #what-the-process-is

In short, to get a major feature added to Lingua Franca, one must first get the
RFC merged into the RFC repository as a markdown file. At that point the RFC is
"active" and may be implemented with the goal of eventual inclusion into Lingua
Franca.

  - Fork the RFC repo [RFC repository]
  - Copy `0000-template.md` to `text/0000-my-feature.md` (where "my-feature" is
    descriptive). Don't assign an RFC number yet; This is going to be the PR
    number and we'll rename the file accordingly if the RFC is accepted.
  - Fill in the RFC. Put care into the details: RFCs that do not present
    convincing motivation, demonstrate lack of understanding of the design's
    impact, or are disingenuous about the drawbacks or alternatives tend to
    be poorly-received.
  - Submit a pull request (the PR title should start with "RFC:"). As a pull
    request the RFC will receive design feedback from the larger community, and
    the author should be prepared to revise it in response.
  - Now that your RFC has an open pull request, use the issue number of the PR
    to update your `0000-` prefix to that number.
  - Build consensus and integrate feedback. RFCs that have broad support are
    much more likely to make progress than those that don't receive any
    comments. Feel free to reach out in particular to get help identifying
    stakeholders and obstacles.
  - The team will discuss the RFC pull request, as much as possible in the
    comment thread of the pull request itself. Offline discussion will be
    summarized on the pull request comment thread.
  - RFCs rarely go through this process unchanged, especially as alternatives
    and drawbacks are shown. You can make edits, big and small, to the RFC to
    clarify or change the design, but make changes as new commits to the pull
    request, and leave a comment on the pull request explaining your changes.
    Specifically, do not squash or rebase commits after they are visible on the
    pull request.
  - At some point, a member of the team will propose a "motion for final
    comment period" (FCP), along with a *disposition* for the RFC (merge, close,
    or postpone).
    - This step is taken when enough of the tradeoffs have been discussed and a rough consensus has been reached, so that
      the core team is in a position to make a decision. That does not require
      consensus among all participants in the RFC thread (which is usually
      impossible). However, the argument supporting the disposition on the RFC
      needs to have already been clearly articulated, and there should not be a
      strong consensus *against* that position. Team members use their best
      judgment in taking this step, and the FCP itself ensures there is ample
      time and notification for stakeholders to push back if it is made
      prematurely.
    - For RFCs with lengthy discussion, the motion to FCP is usually preceded by
      a *summary comment* trying to lay out the current state of the discussion
      and major tradeoffs/points of disagreement.
    - The motion for FCP should include a checkbox list that tags major
      stakeholder and reviewers that need to sign off on the FCP. This is often
      the point at which many team members first review the RFC in full depth.
      Once a majority of reviewers approve (and at most 2 approvals are outstanding),
      the FCP begins.
  - The FCP lasts ten calendar days, so that it is open for at least 5 business
    days. It is also advertised widely,
    e.g. on the LF mailing list. This way all
    stakeholders have a chance to lodge any final objections before a decision
    is reached.
  - In most cases, the FCP period is quiet, and the RFC is either merged or
    closed. However, sometimes substantial new arguments or ideas are raised,
    the FCP is canceled, and the RFC goes back into development mode.

## The RFC life-cycle
[The RFC life-cycle]: #the-rfc-life-cycle

Once an RFC becomes "active," anyone may submit an implementation of the
feature as a pull request to the relevant repo. Being "active" is not a rubber
stamp, and in particular still does not mean the feature will ultimately be
merged; it does mean that in principle all the major stakeholders have agreed
to the feature and are amenable to merging it.

Furthermore, the fact that a given RFC has been accepted and is "active"
implies nothing about what priority is assigned to its implementation, nor does
it imply anything about whether a developer has been assigned the task of
implementing the feature. While it is not *necessary* that the author of the
RFC also write the implementation, it is by far the most effective way to see
an RFC through to completion: authors should not expect that other project
developers will take on responsibility for implementing their accepted feature.

Modifications to "active" RFCs can be done in follow-up pull requests. We
strive to write each RFC in a manner that it will reflect the final design of
the feature; but the nature of the process means that we cannot expect every
merged RFC to actually reflect what the end result will be at the time of the
next release.

In general, once accepted, RFCs should not be substantially changed. Only very
minor changes should be submitted as amendments. More substantial changes
should be new RFCs, with a note added to the original RFC. Exactly what counts
as a "very minor change" is up to the team to decide.

## Reviewing RFCs
[Reviewing RFCs]: #reviewing-rfcs

While the RFC pull request is up, the core developers may schedule meetings with
the author and/or relevant stakeholders to discuss the issues in greater detail,
and in some cases the topic may be discussed at a regular LF meeting. In either
case a summary from the meeting will be posted back to the RFC pull request.

Final decisions about RFCs are made after the benefits and drawbacks are well
understood. These decisions can be made at any time. When a decision is made,
the RFC pull request will either be merged or closed. In either case, if the
reasoning is not clear from the discussion in the thread, the a comment
describing the rationale for the decision will be added.


## Implementing an RFC
[Implementing an RFC]: #implementing-an-rfc

Some accepted RFCs represent vital features that need to be implemented right
away. Other accepted RFCs can represent features that can wait until some
arbitrary developer feels like doing the work. Every accepted RFC has an
associated issue tracking its implementation in the relevant repository.

The author of an RFC is not obligated to implement it. Of course, the RFC
author (like any other developer) is welcome to post an implementation for
review after the RFC has been accepted.

If you are interested in working on the implementation for an "active" RFC, but
cannot determine if someone else is already working on it, feel free to ask
(e.g. by leaving a comment on the associated issue).

## RFC Postponement
[RFC Postponement]: #rfc-postponement

Some RFC pull requests are tagged with the "postponed" label when they are
closed (as part of the rejection process). An RFC closed with "postponed" is
marked as such because we want neither to think about evaluating the proposal
nor about implementing the described feature until some time in the future, and
we believe that we can afford to wait until then to do so.
Postponed pull requests may be re-opened when the time is right. We don't have
any formal process for that, you should ask members of the core team.

Usually an RFC pull request marked as "postponed" has already passed an
informal first round of evaluation, namely the round of "do we think we would
ever possibly consider making this change, as outlined in the RFC pull request,
or some semi-obvious variation of it." (When the answer to the latter question
is "no", then the appropriate response is to close the RFC, not postpone it.)


## License
[License]: #license

TODO

---

Text to be placed in `0000_template.md`:

---

- Feature Name: (fill me in with a unique ident, `my_awesome_feature`)
- Start Date: (fill me in with today's date, YYYY-MM-DD)
- RFC PR: [lf-lang/rfcs#0000](https://github.com/lf-lang/rfcs/pull/0000)
- Tracking Issue(s): [lf-lang/lingua-franca#0000](https://github.com/lf-lang/lingua-franca/issues/0000)

# Abstract
[abstract]: #abstract

One paragraph explanation of the feature.

# Motivation
[motivation]: #motivation

Why are we doing this? What use cases does it support? What is the expected outcome?

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

Explain the proposal as if it was already included in the language and you were teaching it to another Lingua Franca programmer. That generally means:

- Introducing new named concepts.
- Explaining the feature largely in terms of examples.
- Explaining how programmers should *think* about the feature, and how it should impact the way they use Lingua Franca. It should explain the impact as concretely as possible.
- If applicable, provide sample error messages, deprecation warnings, or migration guidance.
- If applicable, describe the differences between teaching this to existing Lingua Franca programmers and new Lingua Franca programmers.
- Discuss how this impacts the ability to read, understand, and maintain Lingua Franca code. Code is read and modified far more often than written; will the proposed feature make code easier to maintain?

For implementation-oriented RFCs (e.g. for compiler or runtime internals), this section should focus on how compiler or runtime contributors should think about the change, and give examples of its concrete impact.

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

This is the technical portion of the RFC. Explain the design in sufficient detail that:

- Its interaction with other features is clear.
- It is reasonably clear how the feature would be implemented.
- Corner cases are dissected by example.

The section should return to the examples given in the previous section, and explain more fully how the detailed proposal makes those examples work.

# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not choosing them?
- What is the impact of not doing this?
- If this is a language proposal, could this be done in a library instead? Does the proposed change make LF code easier or harder to read, understand, and maintain?

# Prior art
[prior-art]: #prior-art

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- For language, library, runtime, and compiler proposals: Does this feature exist in other programming languages and what experience have their community had?
- For community proposals: Is this done by some other community and what were their experiences with it?
- For other teams: What lessons can we learn from what other communities have done here?
- Papers: Are there any published papers or great posts that discuss this? If you have some relevant papers to refer to, this can serve as a more detailed theoretical background.

This section is intended to encourage you as an author to think about the lessons from other languages and frameworks, and to provide readers of your RFC with a fuller picture.
If there is no prior art, that is fine - your ideas are interesting to us whether they are brand new or if it is an adaptation from solutions.

Note that while precedent set by other languages is some motivation, it does not on its own motivate an RFC.
Please also take into consideration that Lingua Franca sometimes intentionally diverges from common language features.

# Unresolved questions
[unresolved-questions]: #unresolved-questions

- What parts of the design do you expect to resolve through the RFC process before this gets merged?
- What parts of the design do you expect to resolve through the implementation of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?

# Future possibilities
[future-possibilities]: #future-possibilities

Think about what the natural extension and evolution of your proposal would
be and how it would affect the language and project as a whole in a holistic
way. Try to use this section as a tool to more fully consider all possible
interactions with the project and language in your proposal.
Also consider how this all fits into the roadmap for the project
and of the relevant sub-team.

This is also a good place to "dump ideas", if they are out of scope for the
RFC you are writing but otherwise related.

If you have tried and cannot think of any future possibilities,
you may simply state that you cannot think of anything.

Note that having something written down in the future-possibilities section
is not a reason to accept the current or a future RFC; such notes should be
in the section on motivation or rationale in this or subsequent RFCs.
The section merely provides additional information.


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

- RFCs impose a more formal style of designing and discussion new features,
  which potentially could slow down development.
- RFCs could be interpreted as a strict specification and implemented in a
  water-fall style without considering changing requirements and new insights
  gained from the implementation.
- RFCs focus on the design, not an implementation. Thus, active RFCs will rarely
  describe the end-result and need to be adjusted when the feature gets
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

In order to be light-weight, the proposed process only considers the design of a
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
community uses mdbook for this.

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
