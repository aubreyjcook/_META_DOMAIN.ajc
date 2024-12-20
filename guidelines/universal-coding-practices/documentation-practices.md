# Documentation Practices

## Purpose

This is an overview of guidelines regarding documentation practices from a universal outlook with respect to coding and project management.

## Todos

- [ ] Todo 1

## Documentation Guidelines

### Inline Documentation vs External Documentation

Coding documentation appears in two forms: inline documentation and external documentation. Inline documentation is code comments that are written within the code itself. External documentation is written in separate files that are not part of the codebase.

**Inline Documentation**

Essentially any programming language supports comments, which are text written within source code itself using specialized syntax that is not parsed by the compiler or interpreter. This is the foundation of inline documentation, but an additional form of inline documentation is present within data types that the programmer is able to declare meaningful context for, such as variable names, function names, and class names. It's often posited that the application of these two forms of inline documentation can be sufficient to provide a comprehensive understanding of the codebase, which is a practice that is often referred to as "self-documenting code". But this rationale has extreme limitations. Comment syntax is often restrictive, or can create a cluttered description of the source code that can confuse even the most experienced programmers, especially when reading coding languages that they do not have experience with. Additionally, the information that is able to be conveyed in these data types that are able to be declared with meaningful context is highly limited, often since the declaration of valid data types is limited by the programming language syntax. While there is nothing fundamentally wrong with trying to utilize this approach so that the context of source code is more understandable for the programmer, it is not a comprehensive solution to the problem of understanding the codebase, especially when the codebase is large or complex. Inline documentation however is extremely valuable due to it's specific context that is directly embedded into the source code, providing valuable information about even the context if a specific code block within the same line.

**External Documentation**

This is where external documentation is necessary. Basically every major programming language in standard use makes use of some kind of standardized documentation resources that are publicly available. Private institutions often provide their employees with internal documentation as well. While these resources are often comprehensive, they tend to only describe the fundamental behavior of languages.

This introduces another method for creating external documentation that provides the benefits of external documentation along with the specific contextuality of inline documentation. This is the practice of including an external doc directly alongside the source code files themselves. By providing specific line references within this documentation, any part of the code can be notated with a specific context that is directly related to the source code itself.

### Initialization

Documentation initialization involves either setting up a new project with documentation, or adding documentation to an existing project that lacks it. It's generally better to do the prior and avoid the latter.

We will examine both processes separately.

#### New Project Doc Init

When starting a new project, it's best to start with documentation. This is usually an initial document called 'README' that will usually be of .md type, which stands for a markdown file. Older code usually has a README.txt file, which is a plain text file that can be retrofitted to support markdown but won't display it within an enriched editor or IDE.

The most minimal init README.md I have found useful contains the following:

- Project Name
- Project Purpose Statement
- Project Todos
- Pseudo-Code (optional)
- Pseudo-Directory-Structure (optional)
- Config-Init Log

This outlines the most basic approach to actionably beginning a project.

An additional document that may be included with such a project is Software Architecture Document (SAD) or Architecture Design Document (ADD) which is a much more thorough and standardized document that outlines the architecture of the software project. This document is usually created by a software architect or a lead developer. But such a document can be overly elaborate except when exhaustive project management practices are necessary. These practices tend to be more relevant when development work will be comprised of a team and software delivery pipelines are present. The outline I have described is more relevant to a singular developer, or simply as a way of rapid prototyping a project.

#### Template

For a simple project init readme template, see link:

- [Simple Project Init Readme Template](templates/project-init-readme.md)