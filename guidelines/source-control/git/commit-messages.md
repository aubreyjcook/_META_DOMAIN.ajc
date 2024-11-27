# Git Commit Messages

This document is meant to outline the approach used by developer AJC to writing commit messages. The goal is to provide a consistent and informative way to communicate changes to the project, and demonstrate the developers personal understanding and thought process to this aspect of the development process.

## General Categories of Commits

There are a few general categories of commits that are generally seen, we will categorize them between major and minor significance.

### Major Commits

#### Doc Vs Source Commits

There are usually two kinds of commits that occur, changes to project documentation, and changes to source code. Source code changes tend to break down into other hierarchal categories that are more complex, there is especially a difference between 'feature' and 'bugfix' commits, but these two types of changes tend to be the umbrella under which you can expect to categorize most of your commits.

##### Documentation Commits

For the most part, documentation change commits require less complexity. It's good to name the document that was changed and possibly the section, especially if the document is large but well organized using markup.

Tag:

```

update doc: 

```

##### Source Code Commits

Commits related to source code.

Tag:

```

update src: 

```

#### Repo Commits

Repo Commits are when a general change is made to the whole repo, this is usually a combination of both documentation and source code commits, along with potential directory structure changes.

Tag:

```

update repo: 

```

### Minor Commits

#### Directory Structure Commits

Directory structure commits are maintenance commits that are necessary when a file or directory is created, moved, or deleted. These commits are important to keep the project organized and maintainable. Directory structure commits can be highly specific or generalized. If a commit is mainly only a change in directory structure it should be indicated as such.

Tag:

```

update dir: 

```

#### Config Commits

Config commits are related to changes to configuration files, usually related to NPM packages and build scripts.

Tag:

```

update config: 

```