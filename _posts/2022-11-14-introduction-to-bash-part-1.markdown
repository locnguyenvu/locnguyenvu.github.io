---
layout: post
title:  "Introduction to Bash (1)"
date:   2022-11-14 13:33:47 +0700
categories: code-academy
---

## What is Bash

Bash is the shell, or command language interpreter, for the GNU operating system. It currently runs on nearly every version of Unix and a few other operating systems - independently-supported ports exist for MS-DOS, OS/2, and Windows platforms.

Shell is simply a macro processor that executes commands, The term macro processor means functionality where text and symbols are expanded to create larger expressions.

Example:

```
echo 'Hello, world'

cat /etc/password
```

## Reasons to learn Bash?

Communicate with the OS where the Graphical User Interface (GUI) is unavailable. 

Knowing how to use Bash helps you speed up repeatedly processing tasks in a fashion way; it's like many open-source applications on GitHub that get installed with a single command. Think about real life scenario on a project; instead of writing a long instruction in full text, you can write a bash script to help your colleagues set up a dev environment on their local.

## Shell Operation

The first word is the command to invoke, and the following words separated by space " " are arguments passed into the program.

```
echo Hello, world
# Invoke echo program with 2 arguments <Hello,> and <world>
```

Words located between single-quote or double-quote pairs are an argument.

```
echo 'Hello, world'
# Invoke echo program with 1 argument <Hello, world>

echo 'Hello, ''world'
# Invoke echo command with 1 argument <Hello, world>
#- Two strings are concatenated into one before passing to the echo command
```


## Commands

The command is an executable program located in multiple `/bin` directories - these directories are defined in the `$PATH` environment variable

```
echo $PATH
# result: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
# directories are separated by ':'

which echo
# result: /usr/bin/echo
# return the path to the executable program
```

Use option `-h` or `--help` to view command usage


### **`cd`**

Change the shell working directory.

Change the current directory to DIR.  The default DIR is the value of the
HOME shell variable.

```
cd [option...] [dir]
```


### **`ls`**

List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

```
ls [option...] [file]
```

Common option flag

|Flag|Descriptions|
|-|-|
|`-a`, `--all` | do not ignore entries starting with `.` (hidden files, current dir, parent dir)|
|`-A`, `--almost-all` | similar to `-a` but does not include `.` and `..`|
|`-l` | long list format|
|`-r`, `--reverse`|               reverse order while sorting

Long output format

>`drwxr-xr-x    1 root     root          4096 Nov 11 18:03 var`

|\<file-type\>|\<owner-permission\>|\<group-permission\>|\<others-permission\>|
|-|:-:|:-:|:-:|
|`-`: regular file<br/>`l`: symbolic link<br/>`d`: directory<br/>`c`: character-file<br>|`r`: read<br/>`w`: write<br/>`x`: execute| `r`: read<br/>`w`: write<br/>`x`: execute|`r`: read<br/>`w`: write<br/>`x`: execute|

### **`curl`**

curl is a tool for transferring data from or to a server. It supports HTTP, HTTPS, FTP, ... protocols

```
curl [option...] <url>
```

Common option flag

|Flag|Description|
|-|-|
|`-L`, `--location`| If the server return redirection HTTP code 302, follow the redirection|
|`-o`, `--output`  \<file-path\>| Write the output to a file|
|`-O`, `--remote-name` | Write output to a local file named like the remote file we get|
|`--output-dir` \<dir\> | This option specifies the directory in which files should be stored|

```
curl --location -o ~/Download/node-telegram-bot-api.zip https://github.com/yagop/node-telegram-bot-api/archive/refs/heads/master.zip
# Download `node-telegram-bot-api` repo to file ~/Download/node-telegram-bot-api.zip

curl --localtion -O --output-dir ~/Download https://github.com/yagop/node-telegram-bot-api/archive/refs/heads/master.zip
# Download `node-telegram-bot-api` repo to file ~/Download/master.zip
```


### **`uznip`**

Extract zip file

```
unzip [option...] file[.zip] [-d exdir]
```


```
unzip -v ~/Download/main.zip
# View files in zip file ~/Download/main.zip

unzip ~/Download/main.zip -d /workspace
# Extract files to /workspace directory
```

### **`tar`**

GNU 'tar' saves many files together into a single tape or disk archive, and can restore individual files from the archive.

```
tar -cf archive.tar foo bar
# Create archive.tar from files foo and bar

tar -tvf archive.tar
# List all files in archive.tar

tar -xf archive.tar
# Extract all files from archive.tar
```

### **`grep`**

prints lines that contain a match for one or more patterns.

```
grep [option...] [patterns] [file...]
```

Common option flag

|Flag|Description|
|-|-|
|`-i`, `--ignore-case`| Ignore case distinctions in patterns and input data, so that characters that differ only in case match each other|
|`-n`, `--line-number`| Prefix each line of output with the 1-based line number within its input file|
|`-H`, `--with-filename` | Print the file name for each match|
|`-r`, `--recursive` | For each directory operand, read and process all files in that directory, recursively|

```
grep -r -n '"babel-eslint"' node-telegram-bot-api/package.json
# Check if application `node-telegram-bot-api` has dependency on package babel-eslint


ls -l node-telegram-bot-api | grep '.eslintrc'
# Check if repo `node-telegram-bot-api` has eslint configured
```

### **`sed`**

sed is a stream editor. A stream editor is used to perform fundamental text transformations on an input stream (a file or input from a pipeline)

```
sed [option...] [sed-command] [file]
```

Common option flag
sed-command follows this syntax:

```
[addr]X[options]
```

X is a single-letter sed command. [addr] is an optional line address. If [addr] is specified, the command X will be executed only on the matched lines. [addr] can be a single line number, a regular expression, or a range of lines

Common commands

|Command|Description|
|-|-|
|`a <text>`| Append text after a line|
|`c <text>` | Replace (change) lines with text|
|`i <text>` | insert text before a line |
|`d` | Delete the pattern space; immediately start next cycle. |


```
sed -n '110p' node-telegram-bot-api/package.json
# Show content on line 110 of file node-telegram-bot-api/package.json

sed -n '110,120p' node-telegram-bot-api/package.json
# Show content from line 110 to 120 of file node-telegram-bot-api/package.json

sed -i '110a Hello world' node-telegram-bot-api/package.json
# Append text after line 110 of file node-telegram-bot-api/package.json
```

> The `s` command (as in substitute) is probably the most important in sed and has a lot of different options. The syntax of the s command is ‘s/regexp/replacement/flags’.

https://github.com/cezerin/cezerin/archive/refs/heads/master.zip


### **`ps`**

report a snapshot of the current processes.

```
ps [option...]
```

Common option
|Flag|Description|
|-|-|
|`-A`, `-e`| Select all processes|
|`-u <userlist>` | Select by effective user ID (EUID) or name.|
|`-C <command>` | Select by command |
|`-f` | Do full-format listing.|
|`-o <format>`| format the output |

```
ps -A u
# List all processes and display user name instead of uid

ps -u postgres uf
# List all processes of user postgres and display in long format

ps -C 'python3 app.py' -o pid h
# Return only PID of process started with command "python3 app.py"
```

---
## Exercise

### Environment setup

1. Docker application
    * [colima](https://github.com/abiosoft/colima)
    * [docker-desktop](https://docs.docker.com/desktop/install/mac-install/)
2. Start new container
    > `docker run -it -p 80:80 --name ubuntu ubuntu:latest`
3. Access the container
    > `docker exec -it ubuntu bash`


### Install PostgreSQL

Install via `apt`

```
# update package info
apt update

apt install -y postgresql
```

Update **PostgreSQL** 2 config files with following information

File `/etc/postgresql/14/main/postgresql.conf`

> `log_destination = 'stderr, csvlog'` \
> `logging_collector = true` \
> `log_filename = '%Y-%m-%d_%H%M%S.log'` \
> `log_directory = '/var/log/postgres'` \
> `log_statement = 'all'` \
> `password_encryption = md5`

File `/etc/postgresql/14/main/pg_hba.conf`

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5

# Allow replication connections from localhost, by a user with the
host    replication     all             127.0.0.1/32            md5
host    replication     all             ::1/128                 md5
```

Use `grep` command to identify the location of the config in the file

```
# Grep pattern from file
grep -n 'log_destination' /etc/postgresql/14/main/postgresql.conf

# Grep pattern from stdin
cat /etc/postgresql/14/main/postgresql.conf | grep -n 'logging_collector'
grep -n log_filename <(cat /etc/postgresql/14/main/postgresql.conf)
```

Use `sed` command to update the config

```
# Preview the update
sed "433a\log_destination = '/var/log/postgresql'" | grep log_destination

# Execute the edit
sed -i "433a\log_destination = '/var/log/postgresql'"
```

Restart the **postgresql** to apply new config

```
# Check application status
service postgresql status

# Restart the application
service postgresql restart
```

Create database for the application
Create a database & user

```
CREATE DATABASE mckca;

CREATE USER mckca WITH PASSWORD 'abc123def';

GRANT ALL PRIVILEGES ON mckca TO mckca;

-- Connect to mckca database
\c mckca;

GRANT USAGE ON SCHEMA public TO mckca;

-- Exit database console
\q
```

### Install application

```
apt install -y curl python3 python3-pip

curl --location https://gitlab.com/crazyfrogs19/mckca/-/archive/main/mckca-main.zip --output /tmp/mckca.zip

unzip /tmp/mckca.zip -d /srv

pip install flask flask-SQLAlchemy python-dotenv psycopg2-binary
```

Replace database info on variable DB_URI configuration in `.env` file

```
sed -i '1c DB_URI=postgresql://mckca:abc123def@localhost:5432/mckca' .env

# OR

sed -i '1s#\(username\|dbname\)#mckca#g' .env
sed -i '1s/password/abc123def/g' .env
```

Run migration

```
psql postgresql://mckca:abc123def@localhost:5432/mckca -f init-db.sql
```

Start application

```
python3 app.py
```

Open browser access link 

* http://localhost:80
* http://localhost:80/todos
* http://localhost:80/users


Use `tail` command to read postgresql logs.

Find clue why the page http://localhost:80/users is slow

```
tail -f /var/log/postgresql/<file-name>.log
```

Find slow query in source code using grep command

```
grep -r -n 'pg_sleep(10)' .
```

Remove slow query by `sed` command

```
sed -i '1d' query1.sql
```