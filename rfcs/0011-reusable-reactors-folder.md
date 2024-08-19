- Start Date: 2024-06-21
- RFC PR: [lf-lang/rfcs#11](https://github.com/lf-lang/rfcs/pull/11)
- Tracking Issue(s): [lf-lang/lingua-franca#0000](https://github.com/lf-lang/lingua-franca/issues/0000)

# Abstract
[abstract]: #abstract

This proposal suggests implementing a standardized project structure for LF programs, featuring a centralized directory for reusable reactors, to enhance development, improve maintainability, and foster collaboration through tools like [Lingo Package Manager](https://github.com/lf-lang/lingo) and [VS Code Extension](https://github.com/lf-lang/vscode-lingua-franca).

# Motivation
[motivation]: #motivation

When developing an LF program, developers often find themselves repeatedly using the same reactors for common operations, such as input/output tasks, data processing, and more. To streamline this process, it is good practice to define LF files containing the reactors' code as a library and import them when necessary. This approach promotes code reuse and consistency across different projects. However, creating numerous LF files with various reactors can lead to a confusing development environment, negatively affecting maintainability and code quality. 

Therefore, establishing a clear and structured project layout is crucial, especially when aiming to share the project with colleagues or the community. The latest features of the [Lingo Package Manager](https://github.com/lf-lang/lingo) enable developers to incorporate other LF projects into their own. This enhancement significantly improves the developer experience by facilitating the use of pre-existing reactors, thereby fostering a more collaborative development environment. By leveraging Lingo, developers can seamlessly integrate and expand upon existing work, which reduces redundancy and accelerates development.

Moreover, the latest developments in the [VS Code Extension](https://github.com/lf-lang/vscode-lingua-franca) include the introduction of a [Lingua Franca Package Explorer](https://github.com/lf-lang/vscode-lingua-franca/blob/extending/LF_PACKAGE_EXPLORER.md). This tool allows users to easily search for and import pre-existing reactors into their current projects. These libraries can originate from two sources: those crafted by the programmer within their local workspace (Local Libraries) and those downloaded using the Lingo Package Manager (Lingo Libraries). This feature significantly enhances productivity and encourages the use of well-tested, modular code components. However, to effectively manage and list these libraries of reactors, it's essential to establish a designated access point within the project structure. This is typically achieved through a centralized directory, ensuring developers can seamlessly access and manage these libraries. This structured approach prevents the non-systematic scattering of libraries throughout the project, promoting organization and ease of maintenance.

Maintaining a fixed structure is crucial because it allows developers to quickly identify where specific components, configurations, and resources are located, which in turn reduces the time spent searching for files and fosters efficient collaboration among team members. Therefore, to achieve these objective, this RFC proposes implementing a clear and standardized project structure for developers to follow. This standardized approach ensures projects are consistently organized, making them easier to comprehend, maintain, and share. Moreover, this structure facilitates the integration of external libraries and tools, thereby fostering a more collaborative development environment.

# Proposed Implementation
[proposed-implementation]: #proposed-implementation

### New project Structure
Currently, the structure of an LF package is organized something like this:

```shell
├── root
│   ├── bin/       # Binary executables
│   ├── include/   # Header files
│   ├── src/       # LF programs
│   ├── src-gen/   # Generated code for local LF programs
│   └── fed-gen/   # Generated code for federated programs (instead of src-gen)
```

To enhance the project structure, the following additions are proposed:

- **`src/lib/`**: Directory for storing reusable reactors.
- **`Lingo.toml`**: Configuration file crucial for project sharing, storing Lingo configuration details including external LF projects to include (refer to [Lingo Documentation](https://github.com/lf-lang/lingo?tab=readme-ov-file#the-toml-based-package-configurations) for further details). The internal structure of this file will be discussed in another RFC.

The refined structure will appear as follows:
```shell
├── root
│   ├── bin/
│   ├── include/
│   ├── src/
│   │  ├── lib/ # Directory for storing reusable reactors
│   │  │   ├── Input.lf # Ex: reactor capturing external inputs (e.g., Microphone, Camera)
│   │  │   └── ComputerVision.lf # Ex: reactor performing computer vision tasks (e.g., object detection, face recognition)
│   ├── src-gen/
│   ├── fed-gen/ # Only for federated programs (instead of src-gen)
│   └── Lingo.toml # Configuration file for Lingo Package Manager
```
The `lib/` directory within the `src/` folder is designated for reusable reactors, making them shareable across multiple projects and minimizing the need for reimplementation. In contrast, the rest of the `src/` folder is reserved for executable programs that may utilize reactors from the `src/lib/` directory during development. Note that the content of directories such as `src-gen`, `fed-gen`, and `include` are outside the scope of this RFC.

This structured approach promotes efficient code reuse, simplifies integration of external resources, and fosters collaborative development practices within the LF community.

### Supporting reactors
[supporting-reactors]: #supporting-reactors

According to this proposal, the `src/lib/` directory is designated for reusable reactors, while the rest of `src/` directory is reserved for executable programs. In some projects, there may be reactors that are used internally by a library but are not intended to be exposed to its users. These can be considered "supporting reactors". Currently, the Lingua Franca language lacks a visibility concept, so there's no clear way to designate certain reactors as internal and not intended for public use at language level. To address this, we propose adopting a convention of using a `private/` directory within the `lib/` folder. This directory can be placed at any level within `lib/`, indicating that its contents are private and should not be exposed to users. It's important to note that this is purely a suggested convention, not something to be enforced at the language or compiler level.

For example:

```shell
├── root
│   ├── bin/
│   ├── include/
│   ├── src/
│   │   ├── lib/
│   │   │   ├── Input.lf
│   │   │   ├── ComputerVision.lf
│   │   │   └── private/
│   │   │       ├── SupportingReactor1.lf
│   │   │       └── SupportingReactor2.lf
│   ├── src-gen/
│   ├── fed-gen/  # Only for federated programs (instead of src-gen)
│   └── Lingo.toml
```

In this structure, the `private/` directory contains `SupportingReactor1.lf` and `SupportingReactor2.lf`. These reactors are intended to be extended by other reactors or imported for specific functions, but are not meant to be exposed to the library’s end users (i.e., developers). This convention helps maintain a clear separation between public and internal components of the library.

# Drawbacks
[drawbacks]: #drawbacks

Here are some potential drawbacks of this proposal:
- **Lack of flexibility**: The proposed structure is rigid and may not be suitable for all projects. It may require additional modifications or extensions to accommodate specific requirements.


# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

### Alternative Names for the `lib/` Folder
The folder containing reusable reactors has been named `lib/` since the idea is to have a centralized location for all reusable reactors that can be imported into other projects, similar to how libraries are used in other programming languages. However, we’re open to discussing different names for this folder if the proposed one causes confusion among developers. Some alternative names that could be considered include:
- `reactors_lib/` or `lfc_reactors/`
- `libraries/`, `lfc_libraries/` or `lfc_lib/`
- `reusables/`
- `modules/`

# Future possibilities
[future-possibilities]: #future-possibilities


