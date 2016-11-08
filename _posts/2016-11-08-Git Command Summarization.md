---
layout: post_layout
title: Git Command Summarization
time: 2016年11月08日 Tuesday
location: 纽约
pulished: true
excerpt_separator: "```"
---

Git is a free and open source distributed version control system. Today, I just want to summarize the most useful and common commands about git.


## Overview

- **Git Initialization**
- **Basic Git Workflow**
- **Using Branches**
- **Resolving Conflicts**
- **Reference**

## 1. Git Initialization

First, we need to onfigure our git client, especially when you use a new computer for the first time. This step will tell your computer who you are, and it is also convenient for a project with multiple contributors.

```
git gonfig --global user.name "your name"
git config --global user.email "your.email@nyu.edu"
```

Then we ned to choose a git hosting like GITHUB to start a new git repository.(create a GITHUB account)

Note:"MIT" license is a popular license for open source code that lets people do whatever they want with your code, as long as they (1) credit you (2) don't hold you liable if something bad happens because they used your code.

If you want to clone or download, use

```
git clone https://github.com/****** my-first-git repo
```

The first argument is the HTTP URL of the repository that you want to clone. The second argument is the name of directory into which git will put your project.(if you didn't specify a name, if will use the repository's name by default)

if you have already had a local project, you want to add it to remote GITHUB account. From inside of the folder, run

```
git init
```

You will see a message indicating you have initialized an empty git repository. Actually,you can use git locally if you just want to use git for version control and don't need to collaborating with others.

However, sometimes we want to have a backup of our work in case we lost our computer or we are not at home. Now we need to create a remote location to our existing repository.

We need to click on "New Repository", then "Create Repository" but don't add a README or LICENSE file. The next screen, github will tel you how to add a remote version to your existing repository.

Also, you can create your remote backup anytime you want. Once your remote repository is established, all the past versions can be retrieved and all the past commits will appear in the remote. 

PS: "Repository" you can call it storehouse, a place where things are kept until needed.

## 2. Basic Git Workflow

Making some changes to your code, then

```
git status
```

will show your modified files, then add the changes to your staging area,

```
git add .
```

Here, "." means all the changed files, you can also specify one files by replacing "." with "filename"
Then we need to conmmit these changes to your repository, use "commit" which means "perform an act". If you commit someone, you are sending that person to an institution. "Commit" alwayse means some problem. You modify your code because there is a problem in it. So using "commit" really makes sense.

```
git commit -m "commit message"
```

Now the changes is committed to your local repository, but not in your remote repository yet, run

```
git push origin master
```

If there is a change in your remote repository, but not in your local copy, you can use

```
git pull origin master
```

to get it. The pipeline is illustrated as follows

<img src="/assets/img/Shell and Git/2.png" width="640px" />

## 3. Using Branches

One of the key strengths of git is the ability to "go back" to a previous "snapshot" of repository. Use

```
git log
```

It will show you all the previous commit. There is a "commit hash" associated with the commit.

```
commit 643e9d57f292***************************
Author: Jun Li <jl7333@nyu.edu>
Date:   Mon Nov 7 12:52:06 2016 -0500

    finish shell
```

Then you can check out that commit into a new branch, like this

```
git checkout -b newbranch 643e9d57f292***************************
```

then run

```
git branch
```

You will find your local repository has two branches, you are currently on newbranch,

```
* newbranch
  master
```

To switch back and forth between branches:

```
git checkout master
git checkout newbranch
```

if you want to restore your master branch back to a previous commit.

```
git checkout master
git revert --no-commit 643e9d57f292..HEAD
git commit
git push origin master
```

If you want to work in new branch,

```
git checkout -b newbranch
git add README.md
git commit -m "starting in new branch"
git push origin newbranch
```

if you want to integrate the change from newbranch into the master branch, use

```
git checkout master
git merge newbranch
git push origin master
```

then delete branch

```
git branch -d newbranch
```

## 4. Resolving Conflicts

Version control systems need some ways to deal with conflicts - figuring out what to do when multiple versions of a file exist. Git will tell you when there is a conflict. For example:

If someone else edit my code on the GITHUB web, I also edit it but in my local repository, when I push it, I will see an error message.

This message tells me what to do next, I need to "git pull before pushing again"

So I first "git pull origin master", it will warn me that there are merge conflicts and I need to fix them.

Then I need to open those files, delete all the conflict markers like ">>>>,======,<<<<<" and edit files the way I want. then push it.

## 5. Reference

1.Scientific Computing Workshop from NYU Tandon Department of ECE

2.Stack Overflow

