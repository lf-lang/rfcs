- Feature Name: `transient-federates`
- Start Date: 2024-03-02
- RFC PR: [lf-lang/rfcs#5](https://github.com/lf-lang/rfcs/pull/5)
- Tracking Issue(s): [Discussion 2212](https://github.com/lf-lang/lingua-franca/discussions/2212)

# Abstract
[abstract]: #abstract

Right now, a federation runs as a single monolithic application, albeit split across machines.  When the federation starts, all federates that will ever join have to join right at the start.  The federation will not start otherwise.
And the assumption is that if any federate leaves the federation, the whole federation should shut down.  There are applications for which that design makes sense, but there are many applications that need more flexibility. This RFC, co-authored with @ChadliaJerad, proposes to introduce the concept of a **transient federate**, a federate that is optionally present or absent.  A federation can start executing without transient federates being present, and transient federates can come and go during execution.

# Motivation
[motivation]: #motivation

Distributed computing fundamentally differs from ordinary programs in a number of ways.
First, the communication between components is via networks, which have potentially ubounded delay and can fail on individual links.
Second, the components themselves can fail independently.
Third, for some applications, the number of components involved in a distributed computation can vary over time.

Visualize, as a canonical example, a distributed bulletin board application, where individual components maintain a history of conversation, there is a requirement for consistency across these components (all users that see common messages see them in the same order), and components can come and go at will (this application is described in detail [here](https://arxiv.org/abs/2109.07771)).
First, there is no requirement that all participants be present for the service to start.
Nevertheless, as participants come and go, the service has to ensure that any two participants that see any two posts see them in the same order (see [here](https://arxiv.org/abs/2109.07771) for anomalies that can result otherwise).

The bulletin board application is by no means general.
It has a special feature that (potentially) all components see all posts made by other components.
Another application that shares such a structure is a distributed database.
There are many more applications, however, where the communication topology is less homogeneous.
Any distributed LF program, for example, could be a candidate if it tolerates components failures. Imagine a network of reactors in a federated program where the failure of a single federate needs to not cause the entire federation to fail.
Such a federate becomes absent and then, possibly, becomes present again after a reboot or a repair.

A major goal of the RFC is to provide a basic underlying mechanism that can lead to such fault tolerance.
It is not a goal of this RFC to provide the mechanisms for detecting failures nor for tolerating extended absences. Arguably, these mechanisms already exist in LF, and, in any case, these mechanisms will be application specific.

The major challenge addressed in this RFC is to ensure that when a (transient) federate joins a federation that is already running, that all federates that interact with this transient federate agree on the tag at which the joining happens.
This is a basic prerequisite for ensuring the kind of consistency indicated in the bulletin board example, where all participants see posts in the same order.


# Goals and Non-Goals
[Goals and Non-Goals]: #goals-and-non-goals

### Goals

The scope of this work is to solve one key problem:  How does a federate join a federation that is already running?  Solving this requires solving the following subproblems:

1. Enabling a federation to start without all federates having connected. Which ones are required for starting and which ones are not?  The answer to this question is application specific, and hence we need to provide syntax for a programmer to distinguish between transient federates and non-transient federates.
2. What does it mean for a federate to "absent"?
3. How do we ensure that when a federate joins, all federates affected by it agree on the tag at which that happened?
4. How should timers in a joining transient federate align with timers in other federates?  Should they start at the time of joining (as in modal models with reset transitions), or should they start as if they had been running since the federation started?
5. How can a federate leave a federation in an orderly fashion without ending the whole federation, and what is the tag at which that happens?

In prototyping transient federates, we realized that they enable a simple "hot swap" mechanism as a small embellishment that follows almost immediately from having solved the problems above.
This mechanism is identical to having a federate join late, with the exception that it replaces an existing federate rather than filling an empty slot.
However, this possibility raises some concerns, explained below.
The "hot swap" mechanism, therefore, is considered a secondary (and perhaps optional) goal.

### Non-Goals

1. The LF language definition includes the notion of a "mutation," which is runtime change in the structure of the program. These are currently only experimentally supported in the TypeScript target. Transient federates are not indended to replace mutations. There is no change in structure implied by a transient federate. Instead, a transient federate can be viewed as a static component with two modes, present and absent. When it is absent, it ignores inputs and produces no outputs. When it is present, it reacts to inputs and possibly produces outputs. The goal of this RFC, therefore, is to define the transitions between these modes.

2. How to ensure that a federate that joins after startup is somehow "valid"?  This problem is no different from how we ensure that a federate that joins at startup is valid.  We have an authentication mechanism, thanks to @hokeun's team, which solves a key part of the problem, but it is only part of the problem.  A more complete solution is needed both at startup and when any transient federate joins.

3. A limited form of "hot swapping" can be realized as a small extension of transient federates.  All that is required is for a transient federate to join even when there is already a running federate in its slot.  The mechanism would be to simply orchestrate an orderly shutdown of the running federate followed by a joining of the replacement. However, this does not come close to providing a true hot-swap mechanism. How does a "hot swapped" component inherit state from the component it is replacing?  This is an important question, but any solution to this problem will be pretty orthogonal to this RFC and could be part of much bigger effort to improve fault tolerance. This RFC could facilitate some experimentation in this direction, where the transfer of state can be left up to the application designer. They just have to store and restore the state of their reactors.

4. How to build an LF application that may, for part of its lifetime, participate in a federation, and for other parts of its lifetime, operate autonomously.  This would be a very interesting extension, but it is out of scope for this RFC.


# Proposed Implementation
[proposed-implementation]: #proposed-implementation

## Program Definition

A federation with transient federates is very similar to an existing federation with the only change being that some federates are marked **transient**.  We propose to do that with an annotation on the instantiation, like this:

```lf
    @transient
    foo = new Foo()
```

This annotation will allow the federation to start without instance `foo` of reactor class `Foo`.
That is, when the RTI has heard from all non-transient federates, it will determine the starting tag and notify all federates that have joined to begin execution at that tag.

## Startup of the Federation

To support this, we propose that the RTI's command-line interface acquire one more option, `-nt` or `--number_of_transient_federates`.  Currently, the RTI takes a command-line argument `-n` or `--number_of_federates` that specifies the total number of federates that it should expect to join the federation.  The `-nt` option will simply specify how many of those are transient.  For example,

```sh
    $ RTI -n 6 -nt 3
```

specifies a total of six federates, three of which are transient.
When a federate joins a federation, it sends a signal of type `MSG_TYPE_FED_IDS` to the RTI.  This signal will need to augmented with at least one bit that identifies whether the federate is transient or persistent.
When the RTI has seen three persistent federates join, it starts the federation as before.

## Joining the Federation


# Drawbacks
[drawbacks]: #drawbacks

Code generation for federations first splits the program into multiple `.lf` files, one for each federate, and then code generates each of those separately.
There is currently nothing that enforces that a federate that joins a federation is actually the program that was code generated when the rest of the federation was code generated.
This can be viewed as either a feature or a bug.
It is a feature because it adds flexibility (e.g., you can use it today to get multilingual federates).
It is a bug because it would be easy for a programmer to mistakenly plug in an incompatible federate into a slot, e.g. one with a different causality interface.

The RFC does not change this vulnerability, but perhaps makes it more tempting to separately compile a federate and then plug it in to an already running federation.
Hence, this feature can be viewed as one that gives users more opportunities for errors.
However, we believe that this problem should be solved for all federated programs, not just transient federates.  One way to solve it would be to fingerprint the generated code for the RTI and the federates together when code is generated.
This would require changing how we currently handle the RTI, making part of the generated program rather than a separate stand-alone program.
Such a change, however, has broad implications and should be handled separately.

One possible partial solution would be to augment the signaling when a federate joins a federation to have it indicate the levels of its input and output ports.  If these do not match what the federation expects, the federate would be rejected.  This will add complexity, however.  It falls into the category of a "bug prevention" mechanism.
It is arguable that this could remain an open issue while we proceed with the RFC because it does not change the semantics.
It merely catches some possible user errors.

# Alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

The LF language design includes the notion of a mutation, which is much more flexible. However, this is only prototyped in the TypeScript target and would probably a hugely complex thing to implement for federations.  This RFC is much more modest.
In short, mutations haven't even been implemented for (most) unfederated execution and appear to be much more complex for federated.
Consequently, this alternative is not currently attractive.

# Unresolved questions
[unresolved-questions]: #unresolved-questions


```
> **Open Question**: Enforcing compatibility.
```




# Future possibilities
[future-possibilities]: #future-possibilities

This is a place to collect further ideas that are beyond the scope of the RFC
topic. This is an optional section and may be left blank. It can be used to
collect ideas that pop up during the review process.

