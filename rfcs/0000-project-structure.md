- Start Date: 2024-06-21
- RFC PR: [lf-lang/rfcs#0000](https://github.com/lf-lang/rfcs/pull/0000)
- Tracking Issue(s): [lf-lang/lingua-franca#0000](https://github.com/lf-lang/lingua-franca/issues/0000)

# Abstract
[abstract]: #abstract

Propose a clear and unambiguous project structure to enhance the deployment of LF projects and ensure support for LF side projects, including the [Lingo Package Manager](https://github.com/lf-lang/lingo) and [VS Code Extension](https://github.com/lf-lang/vscode-lingua-franca).

# Motivation
[motivation]: #motivation

When developing an LF program, developers often find themselves repeatedly using the same reactors for common operations, such as input/output tasks, data processing, and more. To streamline this process, it is good practice to define LF files containing the reactors' code as a library and import them when necessary. This approach promotes code reuse and consistency across different projects. However, creating numerous LF files with various reactors can lead to a confusing development environment, negatively affecting maintainability and code quality. 

Therefore, establishing a clear and structured project layout is crucial, especially when aiming to share the project with colleagues or the community. The latest features of the [Lingo Package Manager](https://github.com/lf-lang/lingo) enable developers to incorporate other LF projects into their own. This enhancement significantly improves the developer experience by facilitating the use of pre-existing reactors, thereby fostering a more collaborative development environment. By leveraging Lingo, developers can seamlessly integrate and expand upon existing work, which reduces redundancy and accelerates development.

Moreover, the latest developments in the [VS Code Extension](https://github.com/lf-lang/vscode-lingua-franca) include the introduction of a [Lingua Franca Package Explorer](https://github.com/lf-lang/lingo/wiki/Lingua-Franca-%E2%80%90-VS-Code-Extension). This tool allows users to easily search for and import pre-existing reactors into their current projects. These libraries can originate from two sources: those crafted by the programmer within their local workspace (Local Libraries) and those downloaded using the Lingo Package Manager (Lingo Libraries). This feature significantly enhances productivity and encourages the use of well-tested, modular code components. However, to effectively manage and list these libraries of reactors, it's essential to establish a designated access point within the project structure. This is typically achieved through a centralized directory, ensuring developers can seamlessly access and manage these libraries. This structured approach prevents the non-systematic scattering of libraries throughout the project, promoting organization and ease of maintenance.

Maintaining a fixed structure is crucial because it allows developers to quickly identify where specific components, configurations, and resources are located, which in turn reduces the time spent searching for files and fosters efficient collaboration among team members. Therefore, to achieve these objective, this RFC proposes implementing a clear and standardized project structure for developers to follow. This standardized approach ensures projects are consistently organized, making them easier to comprehend, maintain, and share. Moreover, this structure facilitates the integration of external libraries and tools, thereby fostering a more collaborative development environment.

# Proposed Implementation
[proposed-implementation]: #proposed-implementation

The current structure of a LF project is organized as follows:

```shell
├── root
│   ├── bin/       # Binary executables
│   ├── include/   # Header files
│   ├── src/       # LF programs
│   ├── src-gen/   # Generated code for local LF programs
│   └── fed-gen/   # Generated code for federated programs (instead of src-gen)
```

To enhance the project structure, the following additions are proposed:

- **lib/**: Directory for storing reusable reactors.
- **Lingo.toml**: Configuration file crucial for project sharing, storing Lingo configuration details including external LF projects to include (refer to [Lingo Documentation](https://github.com/lf-lang/lingo?tab=readme-ov-file#the-toml-based-package-configurations) for further details).

The refined structure will appear as follows:
```shell
├── root
│   ├── bin/
│   ├── include/
│   ├── lib/ # Directory for storing reusable reactors
│   │   ├── Input.lf # Ex: reactor capturing external inputs (e.g., Microphone, Camera)
│   │   └── ComputerVision.lf # Ex: reactor performing computer vision tasks (e.g., object detection, face recognition)
│   ├── src/
│   ├── src-gen/
│   ├── fed-gen/              # Only for federated programs (instead of src-gen)
│   └── Lingo.toml            # Configuration file for Lingo Package Manager
```

When executing `lingo build`, additional directories will be created in the project root:

```shell
├── root
│   ...
│   ├── target/
│   │   ├── lfc_include/      # Includes all downloaded external LF projects
│   │   │   ├── remote_lf_project_1/
│   │   │   └── remote_lf_project_2/
│   │   ├── libraries/
│   │   ├── library/
│   ...
```

The ‘Lingua Franca Package Explorer’ in the VS Code extension demonstrates how to utilize this structure. It presents a TreeView with two sections:
- **Local Libraries**: Lists libraries defined by the programmer, located at `root/lib/*.lf`.
- **Lingo Libraries**: Lists libraries downloaded from the Lingo Package Manager, located at `root/target/lfc_include/remote_lf_project_1/lib/*.lf`.

This structured approach promotes efficient code reuse, simplifies integration of external resources, and fosters collaborative development practices within the LF community.

# Drawbacks
[drawbacks]: #drawbacks

Feel free to suggest potential drowbacks of the proposed strucuture.


# Rationale and alternatives
[rationale-and-alternatives]: #rationale-and-alternatives

### Alternative Names for the `lib/` Folder
The folder containing reusable reactors has been named `lib/` since the idea is to have a centralized location for all reusable reactors that can be imported into other projects, similar to how libraries are used in other programming languages. However, we’re open to discussing different names for this folder if the proposed one causes confusion among developers. Some alternative names that could be considered include:
- `reactors_lib/` or `lfc_reactors/`
- `libraries/`, `lfc_libraries/` or `lfc_lib/`
- `reusables/`
- `modules/`

### Alternative Design Approach
Another approach could involve placing the lib/ folder under the src/ directory, as illustrated below:
```shell
├── root
│   ├── ...
│   ├── src/
│   │   ├── lib/ # Directory for storing reusable reactors
│   │   ├── Main.lf
│   └── ...
```
This structure nests the lib/ folder within src/, organizing reusable reactors alongside other source files. This approach may enhance clarity and organization, but it may also lead to potential conflicts or inconsistencies in the codebase.

# Unresolved questions
[unresolved-questions]: #unresolved-questions

Here are some questions worth considering:

1. Do you think it's beneficial to have a separate `lib/` directory for reusable reactors?

2. What are your thoughts on the directory structure generated by the `lingo build` command?
    - Does it facilitate clarity, or does it lead to confusion?
    - If you find it confusing, how would you propose improving it?

# Future possibilities
[future-possibilities]: #future-possibilities


