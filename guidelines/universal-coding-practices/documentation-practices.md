# Documentation Practices

## Purpose

This is an overview of guidelines regarding documentation practices from a universal outlook with respect to coding and project management.

## Documentation Guidelines

### Inline Documentation vs External Documentation

Coding documentation appears in two forms: inline documentation and external documentation. Inline documentation is code comments that are written within the code itself. External documentation is written in separate files that are not part of the codebase.

**Inline Documentation**

Essentially any programming language supports comments, which are text written within source code itself using specialized syntax that is not parsed by the compiler or interpreter. This is the foundation of inline documentation, but an additional form of inline documentation is present within data types that the programmer is able to declare meaningful context for, such as variable names, function names, and class names. It's often posited that the application of these two forms of inline documentation can be sufficient to provide a comprehensive understanding of the codebase, which is a practice that is often referred to as "self-documenting code". But this rationale has extreme limitations. Comment syntax is often restrictive, or can create a cluttered description of the source code that can confuse even the most experienced programmers, especially when reading coding languages that they do not have experience with. Additionally, the information that is able to be conveyed in these data types that are able to be declared with meaningful context is highly limited, often since the declaration of valid data types is limited by the programming language syntax. While there is nothing fundamentally wrong with trying to utilize this approach so that the context of source code is more understandable for the programmer, it is not a comprehensive solution to the problem of understanding the codebase, especially when the codebase is large or complex. Inline documentation however is extremely valuable due to it's specific context that is directly embedded into the source code, providing valuable information about even the context if a specific code block within the same line.

**External Documentation**

This is where external documentation is necessary. Basically every major programming language in standard use makes use of some kind of standardized documentation resources that are publicly available. Private institutions often provide their employees with internal documentation as well. While these resources are often comprehensive, they tend to only describe the fundamental behavior of languages.

This introduces another method for creating external documentation that provides the benefits of external documentation along with the specific contextuality of inline documentation. This is the practice of including an external doc directly alongside the source code files themselves. By providing specific line references within this documentation, any part of the code can be notated with a specific context that is directly related to the source code itself.