# Agent Instructions (Template)

## Purpose

This is a reusable directive for an agentic AI working inside a repository. Copy it into a project and replace the `{{...}}` placeholders before use. It is the "main directive" an agent should re-check whenever it is given a new set of instructions, in case the file has been updated.

If the agent faces a major decision or an open question, it should ask for clarification rather than guess.

## Agenda

### Main

This is `{{software}}`, written or built in `{{technology}}`.

### Documentation

There should be thorough documentation for how to use the program/software, plus documentation written specifically for someone who understands programming fundamentals and is trying to learn by understanding the code. This means documented overviews and detailed inline comments within the codebase itself.

### Versioning

Create versions in directories named `v0_0x` (e.g. `v0_01`, `v0_02`, `v0_03`, …). Since this is alpha, each version should include documentation of what changed — again with the intent of demonstrating, to someone learning the underlying language/technology, how the code works and how to use the software.

Whenever a new version is finished, document a roadmap for the next version.

### Notes

There may be additional agent instructions embedded in nested directories; check for them.
