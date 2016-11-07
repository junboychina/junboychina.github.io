---
layout: post_layout
title: Shell Programming and Git
time: 2016年11月07日 Monday
location: 纽约
pulished: true
excerpt_separator: "```"
---
## Overview

1. **Shell Command Line**
   - **Basic Bash Shell**
   - **Manipulate Output**
   - **Loops and Conditionals**
   - **Workig with Remote Server**
   - **A Simple Practice Task:Sort CitiBike Data**
2. **Git**
   - **Basic Git Workflow**
   - **Using Branches**
   - **Resolving Conflicts**
3. **Reference**

## 1-1. Basic Bash Shell

First, enter your "Terminal". The most useful and common commands as follows:
- **jl7333@server:~$** : it shows my username(jl7333), hostname(server),my current working directory(~), showing that I am working as a normal(unprivileged) user.
- **ls** :list the contents of the directory you are in. (ls -l, ls -lt, ls -ltr, you can check using ls --help)
- **pwd** :print working directory
- **mkdir directory_name** :create a new directory inside our current directory
- **cd directory_name** :change directory
- **cd** :takes you to your home directory
- **cd ..** :takes you to the one level higher directory
- **/home/jl7333** : using "/" can give us a full path
- **nano filename.txt** :create a file, open it for editing
- **cat filename.txt** :see the contents of a file, print the contents of the file to the terminal output
- **cp filename1.txt filename2.txt** :copy a file from file1 to file2
- **mv filename1.txt mycopy.txt** :move(or rename) a file
- **rm mycopy.txt** :delete a file (Be very, very careful!)
- **echo** :to print a quoted string to the terminal output
- **a="hello world"** :saving string in a variable, echo $a, will print "hello world"
- **history** :see your command history all at once.
- **!1** :run a command that appears as number 1 in your history

