---
layout: post
title: (not) tracking config-files with git
tags: git
---

A typical project scenario is, that there are some config files which should be tracked with git - but only with their default/initial values.  
For production and/or testing any changes on these config-files should not be tracked.

The first hasty idea would be to use `.gitignore`, but once a file is tracked any changes will be recognized by git.  
The solution is `update-index` or `update-index --skip-worktree`!

To ignore all current changes made to `my-file.config` 
```
git update-index my-file.config
```

To ignore any changes made to `my-file.config` from now on
```
git update-index --skip-worktree my-file.config
```

When you want to track the file again, call
```
git update-index --no-skip-worktree my-file.config
```

To see which files are currently _untracked_ use
```
git ls-files -v |grep '^H'
```

You will get into a dilemma when you want to switch branches with an untracked file, but `git stash` is your friend.  
We first track the file again, then locally save the changes with `git stash`, switch the branch, do something, switch back, get the changes from `git stash`, untrack the file again.

```bash
~/my-project % git checkout master
error: Your local changes to the following files would be overwritten by checkout:
	my-file.config
Please commit your changes or stash them before you switch branches.
Aborting
~/my-project %
~/my-project % git update-index --no-skip-worktree my-file.config
~/my-project % git stash push
Saved working directory and index state WIP on development: cde75e0 a meaningful commit message
~/my-project % git checkout master
Switched to branch 'master'
~/my-project % ./do-something-on-master-branch.sh
~/my-project % git checkout development
Switched to branch 'development'
Your branch is ahead of 'origin/development' by 1 commit.
  (use "git push" to publish your local commits)
~/my-project % git stash list
stash@{0}: WIP on development: cde75e0 a meaningful commit message
~/my-project % git stash pop
~/my-project % git update-index --skip-worktree my-file.config
```

`git stash` saves the changes only locally - no need to be afraid your secret config will be pushed somewhere public.
