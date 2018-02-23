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
