---
layout: post
title:  "Introduction to Bash (2)"
date:   2022-11-21 09:33:47 +0700
categories: code-academy
---

## Redirection
Before executing a command, its input and output may be redirected to a special notation interpreted by the shell. Redirection allows commands’ file handles to be duplicated, opened, closed, made to refer to different files, and can change the files the command reads from and writes to.

**File Descriptors (FD)**

In Linux/Unix, everything is a file. Regular files, directories, and even devices are files. Every File has an associated number called File Descriptor (FD).

When you execute a command from the terminal, there are three files always open `stdin`, `stdout`, `stderr`

|File path|File descriptor|File name|
|-|-|-|
`/dev/stdin` | `0` | Standard input|
`/dev/stdout` | `1` | Standard output|
`/dev/stderr` | `2` | Standard error | 

Examples:
```
ls -lA 
# stdin is the command you "ls -lA" typed from the keyboard
# stdout is list of item in the current dir

ls -lA non-existent-dir
# stdout is empty
# stderr show message like: "non-existent-dir: No such file or directory"
```

Redirection symbol:

|Symbol|Meaning|
|-|-|
| `<` | Redirect input |
| `>` | Redirect output |
| `>>` | Append redirect output |
| `>&` | Redirect output of one file to another |

Examples:

```
cat < /tmp/output.txt
# Redirect the content of file /tmp/output.txt to stdin and cat command read from the stdin

ls -lA > /tmp/output.txt
# Redirect the output of stdout to file /tmp/output.txt

ls -lA >| /tmp/output.txt
# Redirect the output of stdout to file /tmp/output.txt, if file does not exist create it

ls -lA non-existent-dir 2> /tmp/error.txt
# Redirect the message "non-existent-dir: No such file or directory" of sderr to file /tmp/error.txt

ping localhost >> /tmp/ping-result.log
# Continuously append ping result to file /tmp/ping-result.log

ls -lA . non-existent-dir > /tmp/error.log 2>&1
# Redirect both stdout & stderr to file `/tmp/error.log`, list of items and error message appear in the file content
#- Redirect stdout to file /tmp/error.log
#- Redirect stderr to stdout, and the stdout has already redirected to file /tmp/error.log so the error message goes to there

ls -lA . non-existent-dir 2>&1 > /tmp/error.log
# Redirect only stdout to the file `/tmp/error.log`, only list item appears in the file content, and stderr to stdout so the error is printed on the terminal via stdout
```


## Chaining operators

### **`&&`**

AND Operator will execute the second command only if the first command executes successfully

```
command 1 && command 2
```

### **`||`** 

OR Operator is opposite of `&&` Operator. OR Operator will execute the second command only if the First command Fails.

```
command 1 || command 2
```

### **`|`**

PIPE is a kind of operator that can be used to display the output of the first command by taking input to the second command. 

```
command 1 | command 2 <output from command 1>

ps -ef | grep 'python3 app.py'
# 1. ps command print out a list of process
# 2. grep read the output of ps command and print line of process with command 'python3 app.py'

ls -lA /etc | grep passwd
# 1. ls print a item list in directory /etc
# 2. grep read the output of ls command and print line of item 'passwd'
```

### **`&`**

Ampersand Operator is a kind of operator which executes given commands in the background

```
ping localhost &
# Run the ping command in background
```

Start application in the background

```
python3 app.py &
```

Redirect app output to the log file

```
# Create log file
mkdir -p /var/log/flask-example
touch /var/log/flask-example/app.log

# Start app in background and redirect output to log file
python3 app.py >> /var/log/flask-example/app.log 2>&1 &
```

### **`;`**

Semicolon Operator executes multiple commands at once sequentially as it is mentioned and provides output without depending on the success & failure of other commands

Restart the application

```
# Single step
PID=$(ps h -o pid -C 'python3 app.py')
kill $PID
python3 app.py >> /var/log/flask-example/app.log 2>&1 &

# Chain
kill $(ps h -o pid -C 'python3 app.py') ; python3 app.py >> /var/log/flask-example/app.log 2>&1 &
```

## Shell scripts

A shell script is a file that contains one or more commands. Shell scripts provide an easy way to carry out tedious commands, large or complicated sequences of commands, and routine tasks. When you enter the name of a shell script file, the system executes the command sequence contained by the file.

You can create a shell script by using a text editor. Your script can contain both operating system commands and shell built-in commands.

**Write a shell script**

1. Create a file using text editor (vi/vscode/notepad)
2. Start the first line with `#!/usr/bin/env bash`
    > “#!” is an operator called shebang which directs the script to the interpreter location
3. Write commands
4. Save the file with `.sh` extension (ex: myscript.sh)

Example create file `listitem.sh`:

```
#!/usr/bin/env bash

# This is a comment, words following `#` symbol are comment

ls -lA  # This is a comment also
```

**Run the script file**

Option 1:

```
bash listitem.sh
```

Option 2:

```
chmod +x listitem.sh
./listitem.sh
```

### Shell parameters

A parameter is an entity that stores values. It can be a name, a number, or one of the special characters listed below. A variable is a parameter denoted by a name. A variable has a value and zero or more attributes.

A parameter is set if it has been assigned a value. The null string is a valid value. Once a variable is set, it may be unset only by using the unset builtin command.

```
a='Hello, world'
# Assign parameter `a` with value 'Hello, world'

echo $a
# Print 'Hello, world' to the stdout

unset a
# Unset parameter a

echo $a
# Print nothing
```

**Global variable**: Unix system has some built-in variables that can be accessed across all of your scripts (or shells). Run command `env` or `printenv` in the terminal to show them.

* `$HOME`: Home directory of current login user
* `$PATH`: Paths in which the OS look for the executable program (commands)
* `$HOSTNAME`: Name of the computer
* `$PWD`: Path of current directory

**Special parameters**

|Parameter|Description|
|-|-|
|`$?`| Expands to the exit status of the most recently executed foreground pipeline |
|`$$`| Expands to the process ID of the shell |
|`$!`| Expands to the process ID of the job most recently placed into the background, whether executed as an asynchronous command|
|`$_`| Last executed command|

**Shell script arguments**

Arguments can be passed to the script when it is executed, by writing them as a space-delimited list following the script file name.

Inside the script, the $1 variable references the first argument in the command line, $2 the second argument and so forth. The variable $0 references to the current script.

Example: `greeting.sh`

```
#!/usr/bin/env bash

echo 'This is the script filename '$0

echo 'Hello, '$1
```

Run:

```
./greeting.sh McKinsey
# Print following
#>   This is the script filename ./greeting.sh
#>   Hello, McKinsey
```

### Substitution

**${parameter}**

Same as $parameter, i.e., value of the variable parameter. In certain contexts, only the less ambiguous ${parameter} form works. May be used for concatenating variables with strings.

```
word='Hello,'
name='Loc'
greeting=${word}' '${name}
echo $greeting
# Print 'Hello, Loc'
```

**${parameter-default}, ${parameter:-default}**

If parameter not set, use default.

```
DEFAULT_LOG_FILE_PATH='/var/log'

log_filepath=${1:-$DEFAULT_LOG_FILE_PATH}

sed -i "433alog_directory = '${log_filepath}'" /etc/postgresql/14/main/postgresql.conf
```

**${parameter?err_msg}, ${parameter:?err_msg}**

If parameter is set - use it, else return and print the error

```
db_password=${DB_PASSWORD:?'Database password is not set'}
```

**${var:pos:len}**

Expansion to a max of len characters of variable var, from offset pos (starting from zero)

```
parameter=abc123def456

echo ${parameter:0:3}
# Print 'abc'
echo ${parameter:6:3}
# Print 'def'
```

**${var//Pattern/Replacement}**

Global replacement. All matches of Pattern, within var replaced with Replacement.

```
parameter=abc123def456

echo ${parameter//abc/123}
# Print 123123def456
```

## Exercise

Write a installation script for application in [part 1]({% post_url 2022-11-14-introduction-to-bash-part-1 %})

* Uploadload your shell script to (https://gist.github.com)[https://gist.github.com/]
* Use `curl` to download the bash file and execute
    ```
    curl --location https://raw-gist-file.sh | bash
    ```