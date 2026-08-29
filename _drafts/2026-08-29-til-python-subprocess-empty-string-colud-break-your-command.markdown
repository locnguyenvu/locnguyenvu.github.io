---
layout: post
title:  "[TIL] Python subprocess: An empty string could break your command"
date:   2026-08-29 13:33:47 +0700
categories: git shell
---


I'm writing a python script to control the working terminal. I use kitty which support commands for writing script control the terminal, while these function works with shell script, I choose to use the python for better handling the data structure, as result returned by kitty command contents json data, parsing them to python dict is more easier to dealing with multi-level object.

The work flow is, I would create a new tabs then create windows in newly created tabs.

By default kitty when opening new tab it will has a default windows

Given I have 3 windows with given tab here are the code I used to creating tabs and it's windows

Scenario: I create new tab name `tab_title` with 3 windows with name: `window1`, `window2` and `window3`

In bash shell it will be followings

```bash
# create the tab first and set default window title
kitty @ launch --type=tab --tab-title=tab_title --window-title=window1

# open the second and third windows and attach them to the target tab
kitty @ launch --type=window --match=title:tab_title --window-title=window2
kitty @ launch --type=window --match=title:tab_title --window-title=window3

```

then my first version of python code is as following

```python
import subprocess

tab_title = "tab_title"
windows = [
    {'title': 'window1'}, {'title': 'window2'}, {'title': 'window3'}
]

for i in range(len(windows)):
    window_title = windows[i].get('title')
    options = [
        # for the first window, I need to launch the tab first, that's the type=tab
        # for subsequence window type=window
        f"--type={'tab' if i == 0 else 'window'}",
        f"--window-title={window_title}"

        # tab title only set once on first window
        f"--tab-title={tab_title}" if i==0 or "",

        # subsequence window need to be attached with the tab
        f"--match=title:{window_title}" if i>0 or "",
    ]
    subprocess.run(['kitty', '@', 'launch', *options], check=True)
```

before running the python code, I carefully print the command to stdout and double check the command syntax is correct, they works similar to the bash script version.

however, when running the python script, the second and third windows never open attach to the target tab. I countlessly print the command and double check with the bash script. Command output actually works but not the command run with the `subprocess.run`.

I notice behaviour from the `subprocess.run` seems like the actual command proceed is `kitty @ launch --type=window`, and the match options is obmit, resulting the window does not attach to target tab. My hypothesis is the "empty" string should be the thing. So I change the code

```python
...

for i in range(len(windows)):
    window_title = windows[i].get('title')
    options = [
        # for the first window, I need to launch the tab first, that's the type=tab
        # for subsequence window type=window
        f"--window-title={window_title}"
        f"--type={'tab' if i == 0 else 'window'}",
        (
            f"--tab-title={tab_title}" 
            if i==0 or 
            f"--match=title:{window_title}"
        ),
    ]
    ...

```

the command output is match with previous one

Viola, it works. The `empty string` is a thing and one of major reason for the issue

