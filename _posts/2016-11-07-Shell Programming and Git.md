---
layout: post_layout
title: Shell Programming and Git
time: 2016年11月07日 Monday
location: 纽约
pulished: true
excerpt_separator: "```"
---
Today, I want to summarize some useful shell programming and git skills for ease of reference. These are just some basic commands but I used to forget them and have to Google them over and over and over again.

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

- **nano filename.txt** :create a file, open it for editing. (ctrl+o to write, then "enter",then ctrl+x to exit)

- **cat filename.txt** :see the contents of a file, print the contents of the file to the terminal output

- **cp filename1.txt filename2.txt** :copy a file from file1 to file2

- **mv filename1.txt mycopy.txt** :move(or rename) a file

- **rm mycopy.txt** :delete a file (Be very, very careful!)

- **echo** :to print a quoted string to the terminal output

- **a="hello world"** :saving string in a variable, echo $a, will print "hello world"

- **history** :see your command history all at once.

- **!1** :run a command that appears as number 1 in your history

## 1-2. Manipulate Output

First, we need to find some data for practice, for example: run

```
wget https://www.*****.com/README.txt
```

to get data from the Internet. Then use

```
cat README.txt
```

to see the contents of the file. But sometimes the file is too large that you don't want to print all the contents in the terminal output. To see the beginning of the file, use

```
head README.txt
```

To see the tail of the file, use

```
tail README.txt
```

Also, we can specify the number of lines in the output, use

```
head --lines=3 README.txt
tail --lines=3 README.txt
```

To show full page at a time, use (enter "Enter" to scroll through the file, enter "q" to quit)

```
less README.txt
```

To count the number of lines of the output file, use

```
wc -l README.txt
```

To see lines that contain word "book", use

```
grep "book" README.txt
```

To see lines that contain word "book", the save them to a .txt file ( ">" means to redirect the output)

```
grep "book" README.txt > book.txt
```

To find the number of lines that contain word "book", use ("|" means send output of "grep" to "wc" )

```
grep "book" README.txt | wc -l
```

Note: ">" means cover the original content in the file. ">>" means not cover, new data will append to the file instead.

## 1-3. Loops and Conditionals







