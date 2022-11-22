---
layout: post
title:  "Apply Git in software development"
date:   2022-11-22 13:39:47 +0700
categories: code-academy
---

> (*) It is assumed that you have basic git knowledge (branch, commit, working tree, index, repository)

## Introduction

Git is a free, open-source version control system written by Linux author - Linus Torvalds. Git was designed to handle everything from small to massive projects quickly and efficiently. Nowadays, Git plays a vital role in software development; it helps people to collaborate on big projects - organizing their work and maintaining the software's stability.

## Git flow

Git allows and enhances you to have multiple local branches, which can be entirely independent of each other, which means you can have various versions simultaneously in a project. Based on the idea, Vincent Driessen introduced the git branching model to deliver a successful practice in software development.

![git-branch-model](/assets/git-branch-model.png)

### Main branches

The repository holds 2 lifetime branches

**Master** 

Use this branch to manage production environment releases.

**Develop** 

This branch holds the latest state of delivered development change.


Initially, it begins with the branch `master`; you create `develop` branch from `master` and start your development on it. When the source code in the develop branch reaches a stable point and is ready to be released, you merge branch `develop` back to `master`; You name the merge commit id with a version number to keep track. Theoretically, we can set up a Git hook script to trigger the build and roll-out of the software to the production servers.


### Supporting branches

Beside `master` and `develop`, the development model use variety of supporting branches allowing parallel development between team members. We will eventually delete these branches when complete.

**Feature branch**

When you get a new feature, you create a separate branch for your work called the feature branch. Your branch is isolated from other members; you can work on it without corrupting anyone. Ideally, it branches off from `develop` at the beginning, merges back to `develop` when done, and may delete after that.

During development, others may commit their work to `develop` branch; you should pull the latest commit from `develop` frequently to get the updates and resolve conflicts as soon as possible. You may commit many times to save your work on a complicated feature; use option `--no-ff` on merging to `develop` to consolidate all commits; that makes a clean git history on your component.


**Release branches**

Release branches support the preparation of a new production release. They allow for last-minute dotting of i's and crossing t's. Furthermore, they allow for minor bug fixes and prepare the meta-data for a release (version number, build dates, etc.). You create release branches from `develop` at the moment develop almost reflects desired state of the new release.

Ideally, when you branch off `develop`, the `develop` branch should only contain features targeted for the next release; any component not a part of the release should be pending until you create the feature branch. This new branch may exist there for a while; until the release may be rolled out definitely. During that time, bug fixes may be applied in this branch (rather than on the `develop`). Adding significant new features here is strictly prohibited, and they must be merged into `develop`, and wait for the next cycle.

When the state of the release branch is ready to become an actual release, you merge it to the `master` (with `--no-ff` option); the merge commit must be tagged for future reference. Finally, merge the release branch to `develop`, so future releases also contain these bug fixes.


**Hotfix branches**

Hotfix branches are very much like release branches in that they are also meant to prepare for a new production release, albeit unplanned. They arise from the necessity to act immediately upon an undesired state of a live production version. When a critical bug in a production version must be resolved immediately, a hotfix branch may be branched off from the corresponding tag on the master branch that marks the production version.

When finished, the bugfix needs to be merged back into `master`, it also needs to be merged back into `develop`
