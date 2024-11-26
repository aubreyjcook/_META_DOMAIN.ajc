# Git Commit Messages

This document is meant to outline the approach used by developer AJC to writing commit messages. The goal is to provide a consistent and informative way to communicate changes to the project, and demonstrate the developers personal understanding and thought process to this aspect of the development process.

## Doc vs Code Commits

There are usually two kinds of commits that occur, changes to project documentation, and changes to source code. Source code changes tend to break down into other hierarchal categories that are more complex, there is especially a difference between 'feature' and 'bugfix' commits, but these two types of changes tend to be the umbrella under which you can expect to categorize most of your commits.

### Documentation Commits

For the most part, documentation change commits require less complexity. It's good to name the document that was changed and possibly the section, especially if the document is large but well organized using markup.

A good way to indicate this is to lead the commit message as follows:

```

update doc: 

```

### Source Code Commits

A good way to indicate this is to lead the commit message as follows:

```

update src: 

```

### Repo Commits

Repo Commits are when a general change is made to the whole repo, this is usually a combination of both documentation and source code commits, along with potential directory structure changes.

A good way to indicate this is to lead the commit message as follows:

```

update repo: 

```

#### Directory Structure Commits

Directory structure commits are maintenance commits that are necessary when a file or directory is created, moved, or deleted. These commits are important to keep the project organized and maintainable. Directory structure commits can be highly specific or generalized. If a commit is mainly only a change in directory structure it should be indicated as such.

A good way to indicate this is to lead the commit message as follows:

```

update dir: 

```