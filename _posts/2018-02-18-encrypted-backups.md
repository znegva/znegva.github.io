---
layout: post
title: Encrypted backups with rsnapshot and gpg
tags: macOS Debian backups
---

Currently I am using a backup strategy recoursing to rsnapshot and GPG on macOS as
well as on Debian.

## Setup ..
### .. on macOS

`rsnapshot` can be installed using Homebrew, we also need `coreutils` to get GNU
options for some progrmas, mainly `cp`.

```zsh
~ % brew update
~ % brew install coreutils rsnapshot
```

### .. on Debian

rsnapshot can be installed using your preferred package manager

```zsh
~ % apt-get install rsnapshot
```

## Configuration

`rsnapshot` is configured by a config-file, typically called `rsnapshot.conf`.
Here is mine:

```
#################################################
# rsnapshot.conf - rsnapshot configuration file #
#################################################
#                                               #
# PLEASE BE AWARE OF THE FOLLOWING RULES:       #
#                                               #
# This file requires tabs between elements      #
#                                               #
# Directories require a trailing slash:         #
#   right: /home/                               #
#   wrong: /home                                #
#                                               #
#################################################
config_version		1.2

snapshot_root	/Users/martin/rsnapshot/

cmd_cp			/usr/local/bin/gcp
cmd_rm			/bin/rm
cmd_rsync		/usr/bin/rsync
cmd_ssh			/usr/bin/ssh
cmd_logger		/usr/bin/logger

retain			hourly	4
retain			daily	7
retain			weekly	4
retain			monthly	3

verbose			2
loglevel		3
logfile			/Users/martin/rsnapshot/rsnapshot.log
lockfile		/Users/martin/rsnapshot.pid

# Backup local machine
backup			/Users/martin/Documents/		localhost/
backup			/Users/martin/scripts/			localhost/
backup			/Users/martin/Sites/			localhost/
backup			/Users/martin/projects/			localhost/

exclude			*/node_modules/
exclude			*Cache*
exclude			*cache*
exclude			*Lock*
exclude			*.tmp*
exclude			*.DS_Store*
exclude			*/platforms/
exclude			*/output/
exclude			*/output_web/
exclude			*/bolt/
exclude			*/_site/
```

For a full explanation of all configuration options, please see the [`rsnapshot` docs](http://rsnapshot.org/rsnapshot/docs/docbook/rest.html#configuration).

The most important thing to consider is using tabs inside of `rsnapshot.conf` and
end with a slash when backing up a directory!
