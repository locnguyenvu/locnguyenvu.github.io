---
layout: post
title:  "How I track down deleted code block"
date:   2025-08-23 13:33:47 +0700
categories: git shell
---

### Scenario

I got a task to update a CSS class:

``` css
.component-name .container-name {
  padding: 10px;
}
```

By convention, I knew it should be in:

    /path-to-component/scss/component-name.scss

When I opened the file, I found `.component-name` but no sign of
`.container-name`.

The problem:
- I had no idea what screen this was for (small company, no docs).
- I couldn't test it in the UI.
- But I do know how CSS works 😉.

My gut said this task was obsolete and the code had already been
removed. Time to prove it.

------------------------------------------------------------------------

### Step 1. Check the file history

``` bash
git log -n20 --pretty=%H /path-to-component/scss/component-name.scss
```

That gave me the last 20 commit hashes touching this file.


    commit-20__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-19__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-18__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-17__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-16__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ...
    commit-4__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-3__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-2__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    commit-1__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx


------------------------------------------------------------------------

### Step 2. Search commits for the class

Loop through those commits and grep for `.container-name`:

``` bash
git log -n20 --pretty=%H /path-to-component/scss/component-name.scss | \
	while read gc; do git show "${gc}:/path-to-component/scss/component-name.scss" | grep -q 'container-name' && echo $gc; done | \
    head -n1
```

Result:

    commit-16__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

So *commit-16* was the last time this code existed.

------------------------------------------------------------------------

### Step 3. Find where it got removed

If *commit-16* had it, then *commit-17* must be where it was deleted.
Diffing the two confirmed it:

``` bash
git diff commit-16__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx...commit-17__xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx -- /path-to-component/scss/component-name.scss
```

And bingo 🎉 — I found the diff where `.container-name` was deleted.

------------------------------------------------------------------------

### Lesson learned

* Using git show, grep, and diff, I can track when a code block was last present and exactly when it was removed.
* This approach is especially handy in legacy projects where “tribal knowledge” is lost but Git has the answers.