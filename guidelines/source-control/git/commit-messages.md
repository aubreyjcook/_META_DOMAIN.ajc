# Git Commit Messages

This document is meant to outline the approach used by developer AJC to writing commit messages. The goal is to provide a consistent and informative way to communicate changes to the project, and demonstrate the developers personal understanding and thought process to this aspect of the development process.

## Shorthand Commit Messages

Commit messages are constrained to a certain length. I have developed a method of writing messages in the following format to ensure that the message is concise and informative.

```
update [category]: [file/purpose]: [file/section/lines/description]
```

This is a general format, let's describe the different parts of the message.

* `update` or `upd` for short - This is the action that was taken, it is almost always the same. There are some reasons to potentially override this, but based on the description of commit categories I generally expect to use this action. Some alternate actions might be 'init' or 'config' which are likely used at the beginning of a project.
* `[category]` - This is the general category of the commit, many of which are outlined in the following sections.
* `:` - This is a separator between the category and the file or purpose of the commit. Separators like this will be used throughout the message, we describe it here to make it clear that it is a separator.
* `[file/purpose]` - If a commit targets a specific file, it's name is stated here. If the commit is representative of a general change, like 'adding responsive design', then the purpose of the commit is stated here.
* `[file/section/lines/description]` - This is the most detailed part of the commit message, it describes the specific changes made to the file, or the specific section of the file that was changed. If the change is more general, like 'adding responsive design', then a description of the change is stated here. In this section some text symbols can be indicative of particular kinds of actions, for example `+` can be used to indicate an addition, `-` can be used to indicate a deletion, and `*` can be used to indicate a modification.

**Commit Symbol Indicators** <!-- TODO: Add more of these and standardize. -->

* `+` - Indicates an addition.
* `-` - Indicates a deletion.
* `*` - Indicates a modification.
* `!` - Indicates a critical change.


Make a commit message longer than this can run the risk of not meeting the constraint requirements of commit messages.


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