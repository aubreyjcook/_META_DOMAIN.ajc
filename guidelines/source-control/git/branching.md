# Git Branching Guidelines

Guidelines for creating and managing branches in Git repositories.

## Table of Contents

- [Overview](#overview)

## Overview

Branching is a feature in Git repositories that creates a separate line of development for a project.

Any Git repo is initalized with a default branch called `main` or `master`.

### Branching Separations

A typical branching workflow includes creating branches for build, production, and development or deployment depending on the project requirements. There are some other branches that are common, especially with relation to testing, but these typically comprise the main ones that are involved in the direct development pipeline.

To understand the distinctions between these branches, I'll describe the main outlook of each of these types of branches.

**Development:** This branch is the most basic to understand. Typically when one creates any kind of repo, using the main branch is likely to be done in the manner that the development branch is used. However there are some cases where the development branch will be separated from the main, such as when the main branch contains the production or deployment code, but for some reason the workflow calls for the development branch to be secondary.

**Deployment:** This branch is the second most basic to understand. It contains code that is intended to be functional at run time. The most common reason why this branch and the development branch would technically be the same is because the code **does not** require compilation to be run. It may not require interpretation either but this can be a more esoteric case. For either of these cases the development and deployment branches would have no effective reason to exist separately. Nevertheless, if the code does require compilation or interpretation, the code that is in the deployment branch is what is served either to end users or build process services to create a run-time environment.

 **Note:** There is a technical difference between the deployment and what is called the **Live** branch of a project, the live branch is what is served to end users directly and what is actually executed at run time. However, it's not typical to create a branch for live code specifically except in specific testing scenarios. In fact, the code that is executed at run time might be constructed of an entirely different language, such as when C code is compiled to machine code. The nature of the live code has a lot to do with the sub discipline of development that the project resides in, for instance low level languages are expected to have a 'live' runtime that interacts with the kernel and OS, thus it's likely resulting in machine code that is entirely different from the source code, whereas in web development, the code is often interpreted in a browser and therefore has a closer 'fidelity' to it's original source code when it's actually run. This is a typical pattern of the distinction between compiled versus interpreted languages.

**Build:** <!-- TODO: Describe the build branch -->

**Production:** <!-- TODO: Describe the build branch -->

### Local Pre-Branching Directory Structure Planning

Utilizing a multi branch setup like the one previously described creates a need to manage your local directory containing your repos differently than if you only maintained a single main branch.

Normally one would clone the main branch and work within in it, but if you clone the main branch and then need to instantiate one of your other working branches, it cannot be within the same directory as the main branch as it creates conflicts.

To resolve this in a cleaner manner, you can create a containing directory that holds each cloned branch in separate directories nested within.

For example:
    
1. Go into the directory where you normally store most project repos cohesively, or wherever you intend to store the generalized repo for the project.
2. Create a directory like 'repo-{project-name}'.
3. Clone the main branch into the directory you just created.
4. Use the git worktree command 

```
git worktree add ../branch-name branch-name
```
(note the ../ up directory path modifier)

to declare a new branch corresponding to the one you intend to clone (you usually need to make this before hand, and while the name does not need to match the existing branch, this probably saves you trouble) like 'src' in a new directory within the containing directory you just created.
5. Repeat this for any additional branches you have created to work on separately.

The result is a single directory that contains directories that represent the separate branches. The main branch is named after your actual default repo, so to avoid redundant nesting you name the 'root' directory containing each of these separate branch instances 'repo-{project-name}'. And additionally you can identify the main branch that you stored there.

If you want to have a more manual and fine grain control of this process, you can alter the names of the directories you clone directly, with commands:

```
git clone https://github.com/username/repo.git my-custom-directory
```

```
git clone --branch my-branch --single-branch https://github.com/username/repo.git my-custom-directory
```
This allows more specific customization of your directories and can remove the necessity of using the main directory as I described if you would prefer this instead of the simpler method I described.