---
layout: post
title:  "[TIL] Python subprocess: An empty string could break your command"
date:   2026-08-29 13:33:47 +0700
categories: python shell 
---


I'm writing a Python script to control my working terminal. I use [Kitty](https://github.com/kovidgoyal/kitty), which supports commands for scripting terminal control. While these functions work with shell scripts, I chose Python for better data structure handling. The results from Kitty commands contain JSON data, and parsing them into Python dicts is much easier for dealing with multi-level objects than in Bash.

The workflow is: I create a new tab, then create windows inside that newly created tab.

## The Scenario

I want to create a new tab named `tab_title` with 3 windows named `window_1`, `window_2`, and `window_3`.

In Bash shell, it looks like this:

```bash
# Create the tab first and set first window to the tab default window
kitty @ launch --type=tab --tab-title=tab_title --window-title=window_1

# Open the second and third windows and attach them to the target tab
kitty @ launch --type=window --match=title:tab_title --window-title=window_2
kitty @ launch --type=window --match=title:tab_title --window-title=window_3
```

## My first attempt

My first version of the Python code was like this:

```python
import subprocess

tab_title = "tab_title"
windows = [
    {'title': 'window_1'}, 
    {'title': 'window_2'}, 
    {'title': 'window_3'}
]

for i in range(len(windows)):
    window_title = windows[i].get('title')
    
    options = [
        # For the first window, launch the tab first (type=tab)
        # For subsequent windows, type=window
        f"--type={'tab' if i == 0 else 'window'}",
        f"--window-title={window_title}",
        # Tab title only set once on first window
        f"--tab-title={tab_title}" if i == 0 else "",
        # Condition for attaching 2nd, 3rd windows to the target tab
        f"--match=title:{tab_title}" if i > 0 else "",
    ]
    
    subprocess.run(['kitty', '@', 'launch'] + options, check=True)
```

Before running the Python code, I carefully printed the command to stdout and double-checked the syntax. It looked correct and similar to the Bash script version.

However, when running the Python script, the second and third windows never attached to the target tab. I countlessly printed the command and compared it with the Bash script. The printed output looked fine, but `subprocess.run` did not work as expected.

## The Problem: Empty Strings Are Not "Nothing"

I noticed that `subprocess.run` seemed to proceed as if the command was just `kitty @ launch --type=window`, and the match options were omitted. My hypothesis was that the empty string was the issue.

To confirm this, I tried explicitly providing an empty string in the argument list, and I successfully reproduced the issue.

```bash
kitty @ launch --type=window "" --match=title:tab_title --window-title=window_2
```

produce the same result with the following python code 

```python
subprocess.run(['kitty', '@', 'launch', '--type=window', '', '--match=title:tab_title', '--window-title=window_2'])
```

Dig deeper, I found reason why this happens:

**In Bash**: If a variable is empty, the shell usually removes it completely during word splitting. It disappears from the command line.

```bash 
EMPTYSTR=""; kitty @ launch $EMPTYSTR --match="title:tab_title"

# has difference behavior with

kitty @ launch "" --match="title:tab_title"
```

**In Python subprocess**: When a list passed, Python does not clean anything up. It passes every item in the list directly to the system. An empty string "" is still a real item in the list.

So, instead of seeing no argument, the Kitty program receives an actual empty argument. Most CLI tools get confused by this. They might think the empty string is a value for a previous flag, or they might stop parsing correctly after seeing it. In my case, the empty string broke the parsing, so the `--match` flag was ignored.

## The Fix

I changed the code to avoid putting empty strings in the list at all. Instead of using inline conditions that produce `""`, I build the list step by step:

```python
import subprocess

tab_title = "tab_title"
windows = [
    {'title': 'window_1'}, 
    {'title': 'window_2'}, 
    {'title': 'window_3'}
]

for i in range(len(windows)):
    window_title = windows[i].get('title')
    
    # Start with common options
    options = [
        f"--type={'tab' if i == 0 else 'window'}",
        f"--window-title={window_title}"
    ]
    
    # Only add specific options when needed
    if i == 0:
        options.append(f"--tab-title={tab_title}")
    else:
        options.append(f"--match=title:{tab_title}")

    subprocess.run(['kitty', '@', 'launch'] + options, check=True)
```

Voila, it works!

## Takeaway

When using `subprocess.run` with a list. Don't use empty strings `""` to mean *no argument*, every item is processed as an argument to the command.

This small difference between how Bash handles empty variables and how Python handles list elements is a common trap when moving shell scripts to Python.

