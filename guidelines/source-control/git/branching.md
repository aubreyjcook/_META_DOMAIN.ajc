# Git Branching Guidelines

Guidelines for creating and managing branches in Git repositories.

## Table of Contents

- [Overview](#overview)

## Overview

Branching is a feature in Git repositories that creates a separate line of development for a project.

Any Git repo is initalized with a default branch called `main` or `master`.

A typical branching workflow includes creating branches for build, production, and development or deployment depending on the project requirements. There are some other branches that are common, especially with relation to testing, but these typically comprise the main ones that are involved in the direct development pipeline.

To understand the distinctions between these branches, I'll describe the main outlook of each of these types of branches.

**Development:** This branch is the most basic to understand. Typically when one creates any kind of repo, using the main branch is likely to be done in the manner that the development branch is used. However there are some cases where the development branch will be separated from the main, such as when the main branch contains the production or deployment code, but for some reason the workflow calls for the development branch to be secondary.

**Deployment:** This branch is the second most basic to understand. It contains code that is intended to be functional at run time. The most common reason why this branch and the development branch would technically be the same is because the code **does not** require compilation to be run. It may not require interpretation either but this can be a more esoteric case. For either of these cases the development and deployment branches would have no effective reason to exist separately. Nevertheless, if the code does require compilation or interpretation, the code that is in the deployment branch is what is served either to end users or build process services to create a run-time environment.

 **Note:** There is a technical difference between the deployment and what is called the **Live** branch of a project, the live branch is what is served to end users directly and what is actually executed at run time. However, it's not typical to create a branch for live code specifically except in specific testing scenarios. In fact, the code that is executed at run time might be constructed of an entirely different language, such as when C code is compiled to machine code. The nature of the live code has a lot to do with the sub discipline of development that the project resides in, for instance low level languages are expected to have a 'live' runtime that interacts with the kernel and OS, thus it's likely resulting in machine code that is entirely different from the source code, whereas in web development, the code is often interpreted in a browser and therefore has a closer 'fidelity' to it's original source code when it's actually run. This is a typical pattern of the distinction between compiled versus interpreted languages.

**Build:** <!-- TODO: Describe the build branch -->

**Production:** <!-- TODO: Describe the build branch -->