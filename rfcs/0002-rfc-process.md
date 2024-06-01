- Start Date: 2024-02-05
- RFC PR: [lf-lang/rfcs#2](https://github.com/lf-lang/rfcs/pull/2)
- Tracking Issue(s): None

# Abstract
[abstract]: #abstract

Establish a Request for Comments (RFC) process. The RFC process is intended to
provide a consistent and controlled path for proposing new features to the
Lingua Franca language and the supporting tools and libraries. The goal is to
allow all stakeholders to participate in the design of new features and to
establish confidence in the direction the language and tooling is involving in.

# Motivation
[motivation]: #motivation

Lingua Franca started as a research project and major contributions to the
language and infrastructure have been mostly driven by the research goals of
individual contributors. While this approach allowed for quickly building a rich
set of features, it also created significant heterogeneity within the project.
To further scale the development effort, to professionalize the code base, and
to establish Lingua Franca as a mature platform, a more uniform and disciplined
approach is required.

# Proposed Implementation
[proposed-implementation]: #proposed-implementation

This RFC proposes to add the following files, which describe the RFC process, to
the RFC repository:

- [README.md](https://github.com/lf-lang/rfcs/blob/rfc-process/README.md)
- [0000-template.md](https://github.com/lf-lang/rfcs/blob/rfc-process/0000-template.md)
- [LICENSE](https://github.com/lf-lang/rfcs/blob/rfc-process/LICENSE)

# Prior art
[prior-art]: #prior-art

- The proposed RFC process is heavily inspired (this is not to say shamelessly copied) from the [Rust community](https://github.com/lf-lang/rfcs)
- Python uses [PEP (Python enhancement proposals)](https://peps.python.org/pep-0001/), which is similar to the idea of RFCs but slightly more formal.
- The IETF RFC process is explained [here](https://www.ietf.org/standards/process/informal/).
- Many companies use RFCs internally. For instance Uber has used RFCs sucessfully to scale from a small team to a thousand engineers as described [here](https://blog.pragmaticengineer.com/scaling-engineering-teams-via-writing-things-down-rfcs/).
- [This blog-post](https://jacobian.org/2023/dec/5/how-to-decide/) notes that key for the success of the RFC process is a clear decision making process.

# Unresolved questions
[unresolved-questions]: #unresolved-questions

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

- The proposed process and template are merely a starting point. It can (and
  should) be adjusted to our needs once we gain more experience.
