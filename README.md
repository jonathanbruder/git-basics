# Git basics
**Basic git operations for collaborative development**

If you are just trying to do a thing, [skip to Git in Practice](#git-in-practice).

## Table of contents
1. [Overview](#overview)
1. [Learn more](#learn-more)
1. [Commits](#commits)
    1. [Commit hashes](#commit-hashes)
    1. [Ahead vs. Behind](#ahead-vs-behind)
1. [Branches](#branches)
    1. [Checkout](#checkout)
    1. [Branch names](#branch-names)
    1. [Merges](#merges)
    1. [Conflicts](#conflicts)
    1. [Treeishes](#treeishes)
1. [Tags](#tags)
    1. [Semantic versioning](#semantic-versioning)
1. [GitFlow](#gitflow)
    1. [Special branch names](#special-branch-names)
    1. [Release cycle](#release-cycle)
    1. [Release diagram](#release-diagram)
1. [Git in practice](#git-in-practice)
    1. [Git clients](#git-clients)
    1. [The working tree](#the-working-tree)
    1. [Fetching changes](#fetching-changes)
    1. [Checking out a branch](#checking-out-a-branch)
        1. [Checking out a branch in SourceTree](#checking-out-a-branch-in-sourcetree)
    1. [Creating a branch](#creating-a-branch)
        1. [Naming branches](#naming-branches)
        1. [Creating a branch in SourceTree](#creating-a-branch-in-sourcetree)
    1. [Making changes](#making-changes)
    1. [Staging your changes](#staging-your-changes)
        1. [Staging your changes in SourceTree](#staging-your-changes-in-sourcetree)
    1. [Committing your changes](#committing-your-changes)
        1. [Commit messages](#commit-messages)
        1. [Committing your changes in SourceTree](#committing-your-changes-in-sourcetree)
    1. [Pulling changes](#pulling-changes)
        1. [Rebasing](#rebasing)
        1. [Pulling changes in SourceTree](#pulling-changes-in-sourcetree)
        1. [Rebasing changes in SourceTree](#rebasing-changes-in-sourcetree)
        1. [Oops, I have conflicts](#oops,-i-have-conflicts)
    1. [Pushing changes](#pushing-changes)
        1. [Pushing your changes in SourceTree](#pushing-your-changes-in-sourcetree)
    1. [Resolving merge collisions](#resolving-merge-collisions)
        1. [Why was there a collision?](#why-was-there-a-collision?)
        1. [Merge collision syntax](#merge-collision-syntax)
        1. [Collisions in built files](#collisions-in-built-files)
        1. [Marking as manually resolved](#marking-as-manually-resolved)
            1. [Marking as manually resolved in SourceTree](#marking-as-manually-resolved-in-sourcetree)
        1. [Resolving by choosing one of the versions](#resolving-by-choosing-one-of-the-versions)
            1. [Resolving by choosing in SourceTree](#resolving-by-choosing-in-sourcetree)
## Overview

Git is a software versioning and source control system that allows multiple people to work on the same software at the same time. A git project, or 'repository,' is a series of [branches](#branches) containing [commits](#commits). Specific commits (usually within the main branch, [master](#special-branch-names)) can be **tagged** as a release.
 
Example:
```
// this is the current state of the master branch
branch master is at commit 38deg9f2

// this is a pointer to a specific commit, which always has the 
// same set of previous commits in its history
tags/13.3.8 refers to commit 38deg9f2
```

In the above example, even if new commits are added to master, 13.3.8 will always point to 38deg9f2.

This document will help you learn to manage a git repository.

## Learn more
 Git is a powerful tool and there will be exceptions to almost every line of this document. If you want more in-depth knowledge, [introduction to git (youtube, 1h22m11s)](https://www.youtube.com/watch?v=ZDR433b0HJY) is still one of the most useful, detailed guides to git operations.
  
  You can also [read the git documentation](https://git-scm.com/).

## Commits

Git commits represent specific changesets. A commit stores only as much information as it needs to identify its changes; usually it's a diff between the new version and the old version.

 
There are a few ways to add a new commit. 

- The most obvious is to make changes, tell git to check in those changes by 'staging' them, and then commit them. Committing will add all of the staged changes to the state of the repository.
- Merging a source branch into a destination will add the source branch's commits to the destination. Depending on your configuration, it will either immediately create a merge commit, or wait for you to do it when you are good and ready.

### Commit hashes
Commits are currently identified with an MD5 hash of their changeset, or the first 6-8 characters of the hash. Due to the recent discovery of hash collision attacks in the MD5 space, git will eventually allow for more sophisticated hashing algorithms. 

### Ahead vs. Behind
   
A commit or branch is said to be **ahead** of another if it contains commits that are not in the second branch. A commit or branch is said to be **behind** if it is missing commits that are in the second branch. 
   
## Branches

Branches are forks of the source code. From a technical perspective, branches are just a record the history of a series of commits. When you you add commits to a branch, you are appending the new commit to the top that history.

### Checkout

The primary feature of git is the ability to easily update and **checkout** branches on a local instance of the source code. When you checkout a branch, you are telling git to update the local instance of the source code with the contents of the branch. You can also checkout specific commits, and checkout specific files from other branches. If you have made changes, you can checkout the branch's version of the file to roll back your changes.     


### Branch names

Branches are labelled, and when used with some git tools, the branch name and the current commit ID are interchangeable. 


### Merges

A **merge** is when the contents of one branch are patched onto the contents of another.

### Conflicts

**Conflicts** arise when an operation tries to reconcile blocks of code that have changed differently in two different sources. Normally, when comparing branches, git simply selects the code that has changed in one branch or the other. When conflicts happen, git needs a little help.

**Conflicts** most often happen when doing one of the following: 

1. Merging two branches with changes to code in overlapping regions
2. Attempting to switch to a branch containing changes which overlap with staged changes


### Treeishes

"**Treeish**" is just a fancy name for a commit-like reference.  

Multiple branches can be merged into a single destination branch. For this reason, a commit has a tree of ancestor branches: all the branches that led to that point. 

**Treeishes** include the following:
 
 - commits
 - tags (which represent a commit)
 - branches (which have a current state, and consequently represent a commit)
 - a few other special references, like **HEAD**. 
 
 One or more **treeishes** may be provided to narrow the focus of [git commands](https://git-scm.com/). When you do a [checkout](#checkout), you are targeting a specific **treeish**.
 
## Tags
Tags are a way of flagging a specific commit as a release. Tags usually contain a version number (ex: 12.52.1). Once a tag is created, it will always refer to the same commit. 

To repeat the example from the overview,

```
// this is the current state of the master branch
branch master is at commit 38deg9f2

// this is a pointer to a specific commit, which always has the 
// same set of previous commits in its history
tags/13.3.8 refers to commit 38deg9f2
```

In the above example, even if new commits are added to master, 13.3.8 will always point to 38deg9f2.

### Semantic versioning
It's good practice to use [semantic versioning](//www.semver.org) to generate version numbers. Each section has a meaning.

In 12.52.1,

- "12" is the major version number. This number is only incremented when backwards-incompatible changes are introduced.
- "52" is the minor version number. This number is only incremented when major features are added or significant, backwards-compatible changes are made.
- "1" is the point version number. This number is only incremented when minor, backwards-compatible changes are made. 


## GitFlow 

There several patterns for using git for effective source control management, but one of the most popular and usable is called GitFlow.

 
### Special branch names 

Gitflow has two branches that are always present:
- **Master** - contains all released features
- **Develop** - contains all completed features

In addition, there are special branch prefix names with meaning:

| prefix | branch from | merge to | usage |
|--------|:------------:|:------:|-------|
| feature- | develop | develop | new development on a feature that does not yet exist |
| release- | develop | master | a completed set of features, ready for final release review | 
| bugfix- | develop | develop | a fix to a bug that can wait for release |
| hotfix- | master | master | a fix to a critical issue that can't wait for the next release |
| release-bugfix- | release- | release- | a fix to an issue in the release candidate |

### Release cycle
The GitFlow release cycle flows like so:
```
 feature -> develop -> release -> master
 <-- ahead                    behind -->
```

### Release diagram
The release process can also be demonstrated with a network diagram. In the diagram below, we've made up some (particularly bad) branch names. Asterisks are [merges](#merges), which almost always involve merging the branch that is ahead into the branch that is behind. 


```
   ^                       
 behind
   |     -------*------- master ------*-----------*-...-------------------------*------->
   |             \                   /           /                             /
   |               -- hotfix-oops --            /                             /
   |                                     release-some-features          release-fixes
   |                                          /                             /
   |     -------*------ develop -------------*-...-*-----------------------*------------>
   |             \                          /       \                     /
   |               -- feature-my-feature --           -- bugfix-my-fix --
   |                
 ahead                  
   v                         ---- time ---->
                                * merge
                   
```

## Git in practice
Git is really complex, but much of its power can be used without reaching very deeply into the toolkit. If you choose to take the time to dig deeper, you will be rewarded with an easier recovery from git errors.

### Git clients
Fortunately, git clients make the work of managing source with git much easier. Here are some excellent client choices: 

- [Tower](https://www.git-tower.com/) (mac and windows)
- [SourceTree](https://www.sourcetreeapp.com/) (mac and windows)
- [GitKraken](https://www.gitkraken.com/) (mac, windows, linux)

### The working tree
The files in the main repository folder are known as the **working tree**. The working tree reflects a branch and any changes that have been made since the last commit. 

### Fetching changes
In addition to the [working tree](#working-tree), git keeps an internal representation of all of the commits, tags, and branches in the repository. Updating this internal representation with files from a remote repository is called **Fetching** a remote. **Fetching** does not change files in the working tree. 

**Note:** Most operations involving the network will automatically **fetch**, and some clients will periodically **fetch** automatically, but it's still a good idea to **fetch** before making a new branch.

### Checking out a branch
In most cases, checking out a branch is straightforward.
 
#### Checking out a branch in SourceTree
1. If you have any changes, [Stage](#staging-your-changes) or [commit](#committing-your-changes) them
2. In the left column, make sure the branches section is expanded by mousing over it and clicking "Show."
3. Double-click the name of a branch to check out
4. Heed any warnings generated.

### Creating a branch
Creating a branch involves forking an existing branch at a certain point, and giving the new branch a label. Most clients will offer to check out the new branch. 

#### Naming branches
Branch names should
1. be concise, skipping unnecessary words
2. be all lowercase, with only letters ([a-z]) numbers ([0-9]) and dashes ([-])
3. follow [GitFlow](#gitflow) naming conventions

Examples:

- A branch containing a new feature with a a carousel widget should be called "feature-carousel-widget" instead of "new-rotating-image-slideshow-feature-branch"
- A bugfix for a thank you message email template should be "bugfix-thank-you-template" instead of "fix-for-unclosed-tag-in-thank-you-email-template"

#### Creating a branch in SourceTree
1. Review the [GitFlow section](#gitflow) above to decide which branch is your starting point, and what to call your branch
1. [Stage](#staging-your-changes) or [commit](#committing-your-changes) any changes

### Making changes
Making changes is as simple as editing files in the [working tree](#the-working-tree), and then committing them. 

1. [Create](#creating-a-branch) or [check out](#checking-out-a-branch) a branch.
2. Make changes to the files. Be sure to build any less or js source changes.
3. [Stage](#staging-your-changes) your changes.
4. [Commit](#committing-your-changes) your changes.
5. [Pull](#pulling-changes) any remote changes (don't rebase).
6. [Push](#pushing-changes) your changes.

**Note:** You should almost never make changes directly to **master** or **develop**. Instead, [create a branch](#creating-a-branch) and [make a pull request](#making-a-pull-request).

### Staging your changes
Before you can commit files, you have to **stage** them for commit. This tells git to track them for future inclusion. You can change branches with staged files, so long as there are no [conflicts](#conflicts).
 
#### Staging your changes in SourceTree
1. Click on 'file status' in the left column
2. Click the checkbox on the left side of each file you want to stage, or the checkbox at the top of the unstaged files section to stage all. If you don't have 'unstaged files,' click the checkbox at the the top of the file list to stage all.
 
### Committing your changes
Before you can commit changes, you'll need to [stage them](#staging-your-changes).

#### Commit messages
Commit messages should 

1. be short (<= 50 characters)
2. reference any issue numbers resolved or partially addressed in the commit
3. describe the *new* state of the repo
4. keep the generation of patch notes in mind
 
 Example messages:
 - "Next button no longer causes refresh (fixes #371)"
 - "Adds calendar widget (addresses #104)"
 - "List items sort correctly (fixes #292)"

#### Committing your changes in SourceTree
1. At the bottom of the window, enter a commit message.
2. Click "Commit" in the bottom right of the window.

### Pulling changes
The process of getting changes for the current branch is called **Pulling** a branch. **Pulling** updates the working tree. If there are any remote changes, they will be [merged](#merges) into the [working tree](#the-working-tree).

#### Rebasing
**Rebasing** is pulling a branch and abandoning any local changes or commits. **Rebasing** causes a branch to "reset" to the state of the remote branch. In most clients, **rebasing** will throw a warning if you have unmerged commits or uncommitted changes.
 
#### Pulling changes in SourceTree
1. At the top of the window, click "Pull."
2. Check that "Remote branch to pull" has the same name as the current branch, or select the correct remote branch.
3. Click "Okay."

#### Rebasing changes in SourceTree
1. At the top of the window, click "Pull."
2. Check that "Remote branch to pull" has the same name as the current branch, or select the correct remote branch.
3. Check the checkbox for "Rebase instead of merge."
4. Click "Okay."

#### Oops, I have conflicts
If you are rebasing and have conflicts, discard your changes.

If you are pulling and have conflicts, just [resolve the conflicts](#resolving-merge-collisions) and try again. 

### Pushing changes
The process of updating the remote repository is called **Pushing**. Pushing is not always allowed, but in a GitFlow scenario, it's usually acceptable to push and pull your own branches or branches where you are sharing work.

#### Pushing your changes in SourceTree
1. [Commit](#committing-your-changes) your changes.
2. [Pull](#pulling-changes) any remote changes.
2. Click "Push" at the top of the window.
3. **Important!!** Uncheck all branches except your current branch.
4. Make sure the "track" checkbox at the right side of the your branch name is checked.
5. Make sure "Push all tags" is unchecked. 
6. Click "Okay."

### Resolving merge collisions
[Merge collisions](#collisions) are a necessary evil in distributed development environments. In most cases, your client will there are merge collisions to be resolved.

If you aren't managing releases, you will most often see merge collisions when you try to [pull](#pulling-changes). 

#### Why was there a collision? 
Git see changes smaller than a whole file in "blocks." Any time the same block was changed in two merging versions of the same file, git will create a merged version of the file, highlighting the changes that need resolution. The conflict might be as small as a single character. 

#### Merge collision syntax

When reviewing a file with conflicts, the conflict will be highlighted like this:

![a merge collision](merge-collision.png)

\* some elements of this example [borrowed from stack overflow](//some elements borrowed from http://stackoverflow.com/questions/7901864/git-conflict-markers)

When resolving conflicts by editing, **be sure to remove the conflict markers**. The code should look like a normal file of its type.

#### Collisions in built files
If a conflict exists *only* in a built file, and not in the source itself (due to timestamping, wrapper changes, etc.), the best course of action is to simply rebuild the file and [mark it as manually resolved](#marking-as-manually-resolved).

#### Marking as manually resolved
Complex changes have to be resolved by editing the file by hand. When that happens, you'll need to let git know you have fixed the conflict.

##### Marking as manually resolved in SourceTree
1. Edit the file to resolve the conflicts.
2. Click "file status" in the left column. 
3. Right- or Control-click the file in question.
4. Click "Resolve Conflicts" from the context menu.
5. Click "Mark resolved."
6. When all conflicts have been resolved, [commit your changes](#committing-your-changes), and if appropriate, [pull](#pulling-changes) and [push](#pushing-your-changes).

**Note:** Even if you have just finished resolving conflicts, someone may have made remote changes while you were working, so you it's still a good idea to pull before you push. 

#### Resolving by choosing one of the versions
One option for resolving a conflict is to simply choose one version or the other. If one version only has spacing changes (or other changes with less value), you can choose the other version and the client will check it out for you.

##### Resolving by choosing in SourceTree
1. Click "file status" in the left column. 
2. Right- or Control-click the file in question.
3. Click "Resolve Conflicts" from the context menu.
5. Click "Resolve using mine" (the local or destination branch) or "Resolve using theirs" (the branch being merged in).
6. When all conflicts have been resolved, [commit your changes](#committing-your-changes), and if appropriate, [pull](#pulling-changes) and [push](#pushing-your-changes).

**Note:** Even if you have just finished resolving conflicts, someone may have made remote changes while you were working, so you it's still a good idea to pull before you push.
