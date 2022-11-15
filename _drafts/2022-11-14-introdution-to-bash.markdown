---
layout: post
title:  "Introduction to bash"
date:   2022-11-14 13:33:47 +0700
categories: code-academy
---

### What is SHELL?

At its base, a shell is simply a macro processor that executes commands. Macro processor means functionality where text and symbols are expanded to create more significant expressions.

A Unix shell is both a command interpreter and a programming language. As a command interpreter, the shell provides the user interface to the rich set of GNU utilities. The programming language features allow these utilities to be combined. Files containing commands can be created and become commands themselves. These new commands have the same status as system commands in directories such as `/bin`, allowing users or groups to establish custom environments to automate their common tasks.


### What is BASH?

Bash is the shell, or command language interpreter, for the GNU operating system. The name is an acronym for the ‘Bourne-Again SHell’, a pun on Stephen Bourne, the author of the direct ancestor of the current Unix shell `sh`, which appeared in the Seventh Edition Bell Labs Research version of Unix.

### Commands

**`ls` command**

Use to discover the system.

List items in the current directory
```
ls 
```

list items with a long format
```
ls -l
```

list items with a long format and hidden files/directories
```
ls -lA
```

Long output format

>`drwxr-xr-x    1 root     root          4096 Nov 11 18:03 var`

|\<file-type\>|\<owner-permission\>|\<group-permission\>|\<others-permission\>|
|-|:-:|:-:|:-:|
|`-`: regular file<br/>`l`: symbolic link<br/>`d`: directory<br/>`c`: character-file<br>|`r`: read<br/>`w`: write<br/>`x`: execute| `r`: read<br/>`w`: write<br/>`x`: execute|`r`: read<br/>`w`: write<br/>`x`: execute|


**`cat` command**

It is used to read the content of a file and print to output

```
cat /etc/shells
```

**`grep` command**

Search for PATTERN in FILEs (or stdin)

Regex [^2]

Find error in source `unknown value for etag function`

Find which files throw `TypeError`

**Shell Operation**

1. Read the input from terminal
2. Break the input into words and operators, obeying the quoting rules
> `echo Hello world` \
> `echo 'Hello world'` \
> `echo 'Hello world' echo 'Hello talents'`

3. Parse tokens into simple & compound commands
> `echo 'Hello world'; echo 'Hello McKinseyee'`

4. Perform the various shell expanasions
> `cat file{1,2,3}.txt` \
> `cat file1.txt file2.txt file3.txt` \
> `cat *.txt`

5. Execute the command
6. Waits for the command to complete and collects its exit status


**Exit status**

`0`: Success

`1`: Error

use `$?` to get exit status of previous command

> `date "+DATE: %Y-%m-%d %H:%M:%S"` \
> `cat non-existent-file.txt`


**Pipline**

A pipeline is a sequence of one or more commands separated by one of the control operators `|` or `|&`

The format for a pipeline is

```
[time [-p]] [!] command1 [ | or |& command2 ] …
```

The output of each command in the pipeline is connected via a pipe to the input of the next command. That is, each command reads the previous command’s output. This connection is performed before any redirections specified by command1.

Two types of output

* `stdout`
* `stderr`


**`grep` command**

It is used to search text and strings in a given file

```

```

Using regex https://cheatography.com/davechild/cheat-sheets/regular-expressions/

**`echo` command**

Use to print content to the console

```
echo "Hello, world"
```

Each command above can be considered as a program; which performs a task. By typing it in the terminal, we send a signal to OS to run the program and do the job we want.

Use `which` command to discover the location of the program.

```
$ which echo
# the output: /usr/bin/echo
```

Use **`ls`** command to discover available programs in the directory `/usr/bin`

```
ls -lA /usr/bin
```

Each file in the directory is executable; try to call the command with full path

```
/usr/bin/echo "Hello, world"
```

How can we use `echo` command without providing the full path to the program? Bash looks into directories in `$PATH` environment variable to find executable programs.

Install the new application [bat](https://github.com/sharkdp/bat) run and explore the program path

### Shell variable

List environment variable

```
env # or printenv
```

Assign variable

```
NAME='Value'
```

Assign variable by the output of other commands

```
now=$(date)
timestone=$(date -d '2022-09-11 12:00:13')
```




### Definition

**`control operator`**

A token that performs a control function. It is a newline or one of the following: `||`, `&&`, `&`, `;`, `;;`, `;&`, `;;&`, `|`, `|&`, `(`, or `)`.

**`metacharacter`**

A character that, when unquoted, separates words. A metacharacter is `space`, `tab`, `newline`, or one of the following characters: `|`, `&`, `;`, `(`, `)`, `<`, or `>`.


### Functions

#### Buildin


[^1] https://tldp.org/LDP/abs/html/io-redirection.html#FTN.AEN17884
[^2] https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet