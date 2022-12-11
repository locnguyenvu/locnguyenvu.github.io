---
layout: post
title:  "Inside the git directory"
date:   2022-11-23 10:33:47 +0700
categories: code-academy
---

## Getting started 

Create plain new directory run `git init`

```
mkdir git-experience
cd git-experience

git init
```

The `git init` command creates a `.git` directory inside the current directory with the following structure

```
.git
├── config
├── description
├── FETCH_HEAD
├── HEAD
├── hooks
│  ├── applypatch-msg.sample
│  ├── commit-msg.sample
├── info
│  └── exclude
├── objects
│  ├── info
│  └── pack
└── refs
   ├── heads
   └── tags
```

The `config` file store setting of the local repository extends from the global config and overwrites the predefined value. You can config different name or emails due to each project.

```
git config user.name "the name you want in this project"
git config user.email "the_email@project.com"

# Set default branch name is "main"
git config init.defaultBranch main
```

Initially, the folder `objects` contains two empty folder `info` and `pack`

## Your first commit

Create a file in the current directory

```
echo 'Hello, world' >| hello.txt

# Then run this command
git hash-object hello.txt

# This command return same result as above
git hash-object <(echo 'Hello, world')
```

The command above returns a hashed string like `a5c19667710254f835085b99726e523457150e03`. This hash string is identical to the file's content; you can copy the file to another computer and run the same command, and the result is similar.

The `hello.txt` currently is in the **working directory**; run the command `git add hello.txt` to add it to the **git staging area**. Now take a look at the directory `.git/objects`.

```
.git/objects
├── a5
│  └── c19667710254f835085b99726e523457150e03
├── info
└── pack
```


A new file was created under file path `a5/c19667710254f835085b99726e523457150e03`; if you remove the forward slash - directory delimiter, you will see the path matches with the hashed string of file content.

Create the first commit

```
git commit -m 'First commit'
```

After that, the folder `.git/objects` may look like this (on your local, new added files may have different name & path)

```
.git/objects
├── 67
│  └── ac38590b37477deff534cc43c90d2e97a5d95a
├── 91
│  └── acfacade0f2304724903f60f5648a5060aa011
├── a5
│  └── c19667710254f835085b99726e523457150e03
├── info
└── pack
```

These git object files are different in types:

```
find .git/objects -type f | awk -F/ '{print $3$4}' | while read hash; do echo $hash - $(git cat-file -t $hash); done

# The result is like
# 67ac38590b37477deff534cc43c90d2e97a5d95a - tree
# a5c19667710254f835085b99726e523457150e03 - blob
# 91acfacade0f2304724903f60f5648a5060aa011 - commit
```

**blob** file is the fundamental data unit; git uses blobs to manage file versions, and the whole system is about blob management. Reading blob file returns the content of file `hello.txt` when you open it:

```
git cat-file blob a5c19667710254f835085b99726e523457150e03

# or

git show a5c19667710254f835085b99726e523457150e03
```

**blob** only stores the content of the file laking information of the filename, so **tree** is used to store metadata of files (name, type, mode) and also used to group files together. 

```
git ls-tree 67ac38590b37477deff534cc43c90d2e97a5d95a
# The result
# 100644 blob a5c19667710254f835085b99726e523457150e03    hello.txt
```

Finally, the **commit** file stores the information of the author who makes changes, the **tree** object containing file changes, and the reference to the previous change.

```
git cat-file -p 91acfacade0f2304724903f60f5648a5060aa011
# The result
# tree 67ac38590b37477deff534cc43c90d2e97a5d95a
# author locnguyenvu <the_email@project.com> 1669268362 +0700
# committer locnguyenvu <the_email@project.com> 1669268362 +0700

# First commit
```

Take a look at the `.git/HEAD`; it stores the reference to the current branch.

```
cat .git/HEAD
# The result
ref: refs/heads/main

cat .git/heads/main
# The result 91acfacade0f2304724903f60f5648a5060aa011
# The commit id of last commit
```

## Moving on to the subsequent updates

Add new line to the `hello.txt` and create new file `greeting/message.txt`

```
echo 'This is the second line' >> hello.txt

mkdir greeting
echo 'Welcome to the firm!' >| greeting/message.txt

# Save the update
git add hello.txt greeting/message.txt
git commit -m "Second commit"
```

Now, in the `.git/objects` it should has two commit files, and the latest commit has a reference to the previous one, follow these reference you are able to trace back the git history

```
find .git/objects -type f | awk -F/ '{print $3$4}' | while read hash; do echo $hash - $(git cat-file -t $hash); done | grep commit
# The result
# 4cee4d23d16db606e06aa6d48776c71c3f7a5cf7 - commit
# 91acfacade0f2304724903f60f5648a5060aa011 - commit

git cat-file -p 4cee4d23d16db606e06aa6d48776c71c3f7a5cf7
# tree 89cbb66331db7f60904fc16357bc9a5536fdc4fc
# parent 91acfacade0f2304724903f60f5648a5060aa011
# author locnguyenvu <loc_nguyen_vu@mckinsey.com> 1669283183 +0700
# committer locnguyenvu <loc_nguyen_vu@mckinsey.com> 1669283183 +0700

# Second commit
```

The **tree** object of the latest commit includes the new version of `hello.txt` and new added `greeting/message.txt`

```
git ls-tree 89cbb66331db7f60904fc16357bc9a5536fdc4fc
# The result
# 040000 tree d29769fe469cb436d18793671c31520537dd2284    greeting
# 100644 blob 54af4235ea528e47478ce8941935a1b0f829b85b    hello.txt

git ls-tree d29769fe469cb436d18793671c31520537dd2284
# The result
# 100644 blob 15a16e750395af14db6260a7eb7e81e2d59fc07e    message.txt
```

To view the change on file `hello.txt`, by comparing the content of 2 blob files

```
diff -y <(git cat-file blob a5c19667710254f835085b99726e523457150e03) <(git cat-file blob 54af4235ea528e47478ce8941935a1b0f829b85b)

# The result show the line "This is the second line" was added on later commit

# Hello, world                         |    Hello, world
#                                      |  > This is the second line
```

The content of file `.git/refs/head/main` now should update to the latest commit id `4cee4d23d16db606e06aa6d48776c71c3f7a5cf7`. Branching is a way to name the commit id; it updates eventually due to the latest commit.