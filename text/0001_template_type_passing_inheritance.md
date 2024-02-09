- Feature Name: `template-type-passing-inheritance`
- Start Date: 2024-02-09
- RFC PR: [lf-lang/rfcs#0001](https://github.com/lf-lang/rfcs/pull/0001)
- Tracking Issue (s): [lf-lang/lingua-franca#0001](https://github.com/lf-lang/lingua-franca/issues/0001)

# Abstract
[abstract]: #abstract

Introduction of Generics in C target has made Lingua-Franca very powerful but as of now the inheritance part of 
lingua-franca is not at par. Inheritance in C target with Generics has some apparent flaws especially in template type
passing and template type naming between base and derived reactors

# Motivation
[motivation]: #motivation

Currently, the typenames of templated arguments between base reactor and derived reactors need to be same and there
is no way of passing an explicit type to the templated argument(s) of base reactor

**typename problem**
```
reactor Base<T1> {
    // reactor definition
}

reactor Derived<T, Y> extends Base<T> {
    // reactor definition
}
```
The above lingua-franca code doesn't compile as `reactor Derived<T, Y> extends Base<T>` is not valid syntax
If we change this to `reactor Derived<T, Y> extends Base` and omit the `<T>` argument the code doesn't compile as well

The only way to fix this problem is if we update the example like this
```
reactor Base<T> {
    // reactor definition
}

reactor Derived<T, Y> extends Base {
    // reactor definition
}
```

**explicit type passing**
``` 
reactor Base<T> {
    // reactor definition
}

reactor Child1<T,Y> extends Base<int> {
    // reactor definition
}

reactor Child2 extends Base<long> {
    // reactor definition
}
```
The above lingua-franca code doesn't compile as `extends Base<typename>` is not supported yet. However, with the support
for passing typename to `Base` reactor explicit types can also be passed to the parent reactor. This would enable the
user to extend a non-generic reactor from a generic reactor and vice versa

# Guide-level explanation
[guide-level-explanation]: #guide-level-explanation

Inheritance in C target is resolved at code generation time but this resolution does not take into account the Generic
type resolution.
With this feature `extends` keyword should be used to resolve the type inferred to the parent reactor and then the
resolution of inheritance should take place

# Reference-level explanation
[reference-level-explanation]: #reference-level-explanation

This could be accomplished by resolving generics before resolving inheritance in the C-Code Generator

# Drawbacks
[drawbacks]: #drawbacks

- template type passing to the parent reactor might require a syntax level change

# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

- Derived reactor should not be bound to the typenames of parent reactors

# Future possibilities
[future-possibilities]: #future-possibilities

With this feature the Generics and Inheritance in the C target will be improved and more closer to how it works in
other languages with support of inheritance and generics (C++ for example)

This would make both features more powerful and complete