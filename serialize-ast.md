- Feature Name: `serialize-ast-unstable`
- Start Date: 2024-02-09
- RFC PR: [lf-lang/rfcs#0000](https://github.com/lf-lang/rfcs/pull/0000)
- Tracking Issue(s): [lf-lang/lingua-franca#0000](https://github.com/lf-lang/lingua-franca/issues/0000)

<!-- # Serialize AST as an Unstable Interface -->

## Abstract

This proposes that we provide a mechanism to serialize the AST and give the option for the
serialized AST to be used as an unstable, not-officially-published interface by which new code
interacts with the legacy code base.

This IS NOT (I mean really, seriously, definitely certainly absolutely NOT):

- a proposal to do a "wholesale port" of the existing code base to another language or build system
- a proposal to port ANY particular part of the existing code base to another language or build system

And it is also NOT

- a proposal to, under any circumstance, strictly require any developer to use this serialization
  feature instead of modifying the existing legacy code base

It IS

- a proposal to keep open the option to implement future changes using this serialization feature,
  and implicitly to suggest that the core team and contributors who can tolerate breaking changes
  are encouraged to use it, for modularity purposes, when doing so makes sense, as determined on a
  case-by-case basis.

## Motivation

The following four observations are relevant:

1. Relative to the size of the main code base (the `lingua-franca` repo), the number of contributors
   is large. These contributors are interested in different platforms, ranging from microcontrollers to
   PRET machines to out-of-order machines that run operating systems such as Linux. They have quality
   standards, with some requiring merely a proof-of-concept (e.g. for a student's thesis) and others
   requiring a pathway toward certification for use in safety-critical systems. They also in many cases
   are each working on their own sub-projects, as researchers, leading to many distinct speculative
   features. The cumulative result of this diversity within a small code base is that it is easy for
   different stakeholders to accidentally break or obfuscate each other's code, or to create merge
   conflicts.
2. More concretely, the fact that the same code has many stakeholders means that in order to make
   changes, people have to run other stakeholders' tests, which test functionality that they do not
   need. We have spent a large amount of time trying to optimize CI in order to reduce this
   problem, but it still slows down development by a lot.
3. The build system is also difficult to maintain. This is because the project must build using both an
   Eclipse/Maven Tycho-based and a Gradle-based build system. There was a large effort by Christian last summer to
   improve this, but it still takes perhaps 30 seconds to compile anything and nothing has been done to
   make it easier to add dependencies. Adding a single dependency can take days or weeks of work if the
   contributor who tries to do it is not already an expert on how to use the Eclipse build system.
4. Currently, we are tied to the JVM. It has been argued that Kotlin is a very good language, so it
   is possible that having to use a JVM language is not a very severe restriction. However, all else
   being equal, it is better to have the option to use other platforms than to be tied to the JVM
   because it is possible that we will want to use other technologies that may not be compatible
   with it. Besides, the JVM comes with inherent inconveniences, such as the inconvenience of
   requiring the user to install and use a compatible JVM version, as well as (more significantly)
   the JVM startup time, which is bad for the user experience of command-line tools unless we
   instantiate the tools as daemons and use them via IPC, which adds complexity.

Observation 1 is not very precise (how does one quantify the likelihood of different teams breaking
each others' code), so it is only important insofar as it clarifies the issues underlying
observation 2.

Observation 2 is the strongest motivation behind this RFC. In order to build modules and test them
fully independently from existing code, it is desirable to have test cases that can be stored on a
hard drive and recovered without having to run the existing code, which may be unreliable, so that
engineers can focus just on the modules under test. It is also desirable for the test cases to have
a human-readable representation so that it is clear what the test vector is for the module under
test. These all point to the conclusion that it would be helpful for test cases to be serializable.

Observation 3 is a good reason to use a toolchain that is

- separate from the current toolchain (because the Gradle/Maven Tycho interaction is specific to us
  and is complex)
- modern (because arguably, some recent build systems and package management tools have been able to
  become even more usable than Maven and Gradle due to their having been developed without the
  requirement that they be compatible with older and possibly worse alternatives like Eclipse and
  Ant)

This makes observation 3 related to observation 4, which is that it is desirable not to be tied to
the ecosystem surrounding Java and the JVM. Serialization is the easiest way to become free from the
constraints of the JVM (the assumption here is that the JNI is probably NOT the easiest way).

## Guide-Level Explanation

Here is an architectural diagram of a prototype that uses this serialization feature. Note that the
essence of this proposal is the serialized representation implemented in the LF language and diagram
server (top left) and in the Rust crate (top right); the rest is only provided as an example of how
the serialization feature could potentially be used.

```
  ┌─────────────────────────────────┐  ┌───────────────────────┐  ┌──────────────────┬─────────────────────────┐
  │ LF Language and│                │  │  vscode-lingua-franca │  │ Third-party tool │   Rust crate            │
  │ Diagram Server │                │  │  VS Code extension    │  │ ─────────────────┘  ────────────┘          │
  │ ───────────────┘                │  │ ────────────────────┘ │  │ information about│                         │
  │                                 │  │                       │  │ the LF program  ◄───  Rust data structures │
  │                                 │  │                       │  │                  │           ▲             │
  │                               ┌─┼──┼─► serialized   ───────┼──► serialized    ───┐           │             │
┌─┼─►  EMF model ──► serialized   │ │  │   representation      │  │ representation   └──► deserialization code │
│ │                  representation │  │      │                │  │                  │                         │
│ └─────────────────────────────────┘  └──────┼────────────────┘  └──────────────────┴─────────────────────────┘
│                                             │
│ ┌───────────────────────────────────────────┼────────────────┐
│ │  File System                              │                │
│ │                                           ▼                │
└─┼───────────────.lf files          .lfp files (can be used   │
  │                                  as test cases)            │
  │                                                            │
  └────────────────────────────────────────────────────────────┘
```

An aspect of a design proposal is important if it is both consequential and difficult to change
later.

The architecture sketched above is not important because although the architecture is consequential,
it is not difficult to change later. Part of the intent behind this RFC is not to enable a
particular architecture, but to make it simple to transition to a different architecture (on the
grounds that "the only constant is change"). For example, it should be straightforward for the
language and diagram server to directly write the serialized representation to the file system, and
to do the communication via the file system instead of via LSP and VS Code commands. Doing this
would change everything about the architecture diagram above, but it would change very little about
the code.

The serialization format is not important because although it is difficult to change later, it is
not consequential. The prototype uses S-expressions as the serialization format because
S-expressions are trivially LL, are easy to generate and parse, and are practical for humans to read
and write. There may be other serialization formats that also have these properties; any such format
would be equally suitable. Note however that any format that requires installing a dependency in
`lfc` could potentially consume a large amount of developer time debugging Maven and Gradle.

The presence of metadata, like span information, is important. All available span information is
serialized and represented alongside the semantically relevant aspects of the AST -- this means the
serial representation mixes the AST with its metadata. This includes source locations (lines and
columns, obtained from NodeModelUtils) in LF files as well as source locations in the serialization
format (for ease of debugging when the serialized LF programs are used as test cases).

However, the exact metadata provided (e.g., whether the original text of the source code should be
included, or whether paths should be absolute versus relative to the package root) should not be
important. The is because the content of the metadata should be easy to change later, by changing
only the serialization and deserialization code that is specific to metadata.

The fact that the deserialization code is implemented in Rust is somewhat important, but it does not
seem to be very controversial. To my knowledge, there is no language that we consider to be better
for developer productivity or for reducing the amount of bugs than Rust, and there are some
languages which are probably worse in these ways. Besides, C and C++ code are free to call into
a Rust library that deserializes and converts the data to C-compatible struct layouts, so
implementing the deserialization in Rust should not be a very serious source of technical lock-in.

The fact that the the deserialization code and code analyses are compiled to WebAssembly is not very
important. This may offer some performance advantages relative to starting a new subprocess for each
query, but it would be easy to change from WebAssembly to something else. It can also be argued that
the overall performance differences between the possible approaches are not large enough to be
very consequential.

It is important that the serialization format be treated as explicitly open to extension,
modification, and retraction as a feature. For example, merging of this RFC and of an associated PR
should not preclude us from adding additional communication protocols, such as ways of feeding error
and warning messages back from a module that consumes data in this format, or more interactive
protocols for interprocess communication. It should also not preclude us from making breaking
changes, such as splitting messages represented in this format into multiple different messages, and
code should be written such that changes of that kind are easy to make and support.

## Reference-level explanation

TODO: Add a reference-level explanation. This may involve converting parts of the guide-level
explanation or waiting for the exact serialization format to be established before writing a
reference-level explanation.

## Drawbacks

### Risk of premature ossification

If this feature exists, even in an undocumented form, there is a risk that users will depend on it
for building tools of their own, thus requiring the LF core team to continue to support this feature
in its initial form indefinitely. Because the initial serial representation will contain potentially
serious design mistakes regardless of the amount of care taken in designing it, and because it may
soon be subsumed by alternatives that are less quick to implement but which otherwise might be
preferable, this could lock us in to yet another piece of technology that we do not want to
maintain.

This drawback can be mitigated by excluding this feature from the release notes and public
documentation and discouraging its use outside of the core team until it has stabilized and
demonstrated its long-term practical utility.

### Introduction of boilerplate

In order to have serialization and benefit from it, one must implement and maintain serialization
and deserialization code, and probably some additional code after deserialization to convert the
data into data structures that are convenient to work with.

This is truly a non-negligible amount of code that does not add features. It has already been shown
using a prototype implementation that this code can be written very quickly (in a matter of days),
but inevitably decisions about the serialization format will have to be made, and the small
decisions to be made about the code will cumulatively use up a lot of time, even if the
serialization feature is only used a little bit.

Besides, the degree of separation enforced by this approach will lead to many existing snippets of
code, such as utility functions, having to be copied or re-implemented as they become needed. This
is duplicate work that violates the DRY guideline.

### Performance

This could be slower than alternative ways of hooking new code into the existing compiler. There are
a few arguments against this criticism:

1. As a general rule, it is wise to wait for performance measurements before considering something
   to be a bottleneck, especially when the supposed performance difference is not asymptotic (we
   already have to write all of the generated code to the file system and hand it off to other
   toolchains, so assuming the amount of generated code is not less than the amount of source code,
   asymptotically the amount of file I/O is not greater).
2. It is possible that LF will be useful to large industrial projects even without the number of
   lines of LF code being large. In other words, LF can be integral to a multimillion-line code base
   even if the amount of LF is not in the millions of lines. This is because LF may just be used as
   a high-level coordination language.
3. Even if serialization is used, that does not mean that it has to be used in the critical path.
   For example, if it is used primarily to store test cases for a module that in practice is likely
   to directly access data structures from other modules instead, then serialization does not have
   performance consequences for end users. LLVM IR is an example of this pattern.
4. It should be possible in the future to evolve the serialization format incrementally to optimize
   for performance. For example, it should be possible to request many different files instead of one
   huge file, which opens the door to parallelization and incrementalization. The format could also
   be extended to additionally allow another serialization format that focuses on structure and does
   not need to contain all the details (probably this would be a lightweight form of our current
   instance graph).

The first of these two arguments is stronger than the second, and I believe that on its own it goes
a long way toward addressing this criticism.

### Correctness

Guarantees provided by a sophisticated type system, such as Rust's, might not be preserved when Rust
is composed with another language.

In the case of serialization (where the ownership of immutable data is simply passed from one
process to another) there are few ways to compromise memory safety, which is more or less the only
guarantee provided by Rust.

More generally, this concern is arguably a red herring: The guarantees provided by a type system
are, as always, true within the areas where the type system is used, subject to certain
preconditions which must be met at the boundaries. The fact that other parts of the system besides
the module of interest do not benefit from that module's type system does not make the type system
any less beneficial to that one module, nor does the need to meet some additional preconditions
within a specific, contained area of the code base make the guarantees about that module
fundamentally less sound than they would be otherwise.

## Prior Art

The architecture of the Chocopy compiler used in some compilers courses separates stages of
compilation using serialization. This is not the norm (for performance reasons) but makes sense for
them for pedagogical reasons. However, as research software, it can be argued that the Lingua Franca
code base has similar needs to that of a pedagogical code base: short ramp-up time, easy
communication and implementation of specific small extensions, freedom for contributors to make
independent choices about how to build their own modules.

Bril is another example of a pedagogical code base that uses (human-readable) serialization for
these reasons.

Note that although these examples happen to be using serialization as an integral part of a
compiler for the purpose of separating compiler stages, this proposal is explicitly not dependent on
the assumption that this will happen in the Lingua Franca code base.

## Rationale and Alternatives

### Alternatives relating to the need for serialization

One observation here is that the different ways of interoperating with code that is somehow
separated from the main code base are not mutually exclusive. It might be pragmatic to, on a
case-by-case basis, choose from many available strategies.

#### Use Kotlin for new code, and use the Kotlin to eventually leave the JVM

Kotlin has compilation targets other than Java bytecode, including native, WebAssembly, and
JavaScript. This partially addresses observation 4.

Advantages:

- It is easy to convert Java code to Kotlin code.
- Java/Kotlin interoperability is very good, which makes incremental porting easy.

Disadvantages:

- Until the full code base is ported to Kotlin, it is not possible to develop outside the JVM using
  this approach. This means that the benefits of doing this (simplification of the build system and
  leaving the JVM) might not be realized for multiple years.
- The hard part of fully porting to Kotlin is that we have Java dependencies, most notably XText and
  the KLighD.
- This does not address the desire to separate out test cases away from the core code base. The core
  code base does parsing, validation, and a small amount of initial lowering (resolving references
  etc.)

#### Use WebAssembly (WASM)

WebAssembly can be called from Java and is supported by Extism. However, it is not clear what data
types can be communicated to WASM modules that are easier to work with than the serialization format
being proposed. Even if such data types do exist and are used instead of strings for
interoperability, this will create technical lock-in on a relatively immature piece of technology
(WASM-Java interop).

#### Use JNI

It seems likely that interfacing in this way between languages with different memory management is
error-prone and difficult. Please add detail here if this is considered to be a serious possibility.

Note that JNI does not on its own solve the problem of interop between languages with different
data formats. As with WASM, it is likely that even if JNI is used, its use would be facilitated by
and compatible with serialization for this reason.

#### Just use the JVM

There is a range of ways to address the situation described in the Motivation section without
leaving the JVM.

##### Focus on improving community norms instead of improving technologies used

By introducing RFCs, it is possible to mitigate the damage that individuals can do by addressing
their needs without considering how it will break the code for other people.

It may also be possible to somehow change community norms about how CI is run in order to make it
less of a burden, but no concrete way to do this is on the table currently.

##### Using the same build system, but different programming tools

The idea might be that the conceptual separation between research and industry-quality code is at
the language boundary between Java and Kotlin, or that using Kotlin in certain places helps to
overcome the limitation of having to write Java.

However, this does not solve the problem of the build system being very complex -- in fact it makes
it even more complex -- and the benefit of strong Java/Kotlin interoperability is also a downside
because inconvenient interoperability is a way of reifying what would otherwise be merely a
conceptual boundary.

##### Using a separate build system

The idea would be that jars are built separately using a completely separate code base and linked in
at the JVM level. This is the most similar of all the alternatives to the serialization approach,
but it does not make it as convenient to build a fully independent and self-contained way of running
tests of side-effect-and-io-free modules, and it does not open as many possibilities for
independence from the JVM.

### Alternatives to the Serialization Technique

It is possible to serialize an EMF model as XML. Example code for doing that can be found
[here](https://stackoverflow.com/questions/29496176/serialisation-of-emf-model-to-xml-file-takes-several-hours).

Advantages:

- The code to do this is already provided, so we do not have to write it.

Disadvantages:

- XML is not very human-readable.
- The format might not contain the right information. For example, it might include absolute paths,
  whereas we might decide we prefer for paths to be relative to the project root. If the format is
  to be serialized as test cases and perhaps even committed, then it is desirable for the
  information it contains to contain no information that is specific to the system on which this
  functionality is used, and if it contains too much information, then it will be difficult for
  humans to find the relevant information.

This approach to serialization and deserialization takes care of the "easy part" for us -- it
automatically provides some serial representation -- but it does not help with the "hard part,"
which is designing a serial representation that contains the appropriate information for our use
case and designing a target in-memory representation to which that serial representation should be
translated.

## Unresolved questions

When does it actually make sense to use this proposed interface?

## Future Possibilities

Proprietary extensions to the Docker support and the k8s support could be implemented in a compiled
language using this proposed serialization feature. This would provide an initial validation of the
idea, much as the tracing plugin API has provided initial validation of the plugin API concept.

In the long term, a gradual transition away from the XText-based language server could be
accomplished by letting WASM modules handle some subset of the LSP requests.

It is also possible that in the long term, this could lead to the development of a new compiler IR
for decoupling code generators or diagram synthesis from the core. However, the motivation for this
RFC stands regardless of any assumption about introduction of new code generators or porting of any
code generators to a different toolchain.
