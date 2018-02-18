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

```conf
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

__For macOS__ please be sure to use the GNU version of `cp` installed with `coreutils`, this is ensured by `cmd_cp			/usr/local/bin/gcp`.

The most important thing to consider is using tabs inside of `rsnapshot.conf` and
end with a slash when backing up a directory!

As you can see my rsnapshot-directory is `/Users/martin/rsnapshot/`, the `rsnapshot.conf` shown above is saved in this directory.

__some of the excludes explained:__

* `exclude			*/node_modules/` to exclude the `node_modules` directory in any `npm` project
* `exclude			*Cache*` etc. to exclude any cached or temp files
* `exclude			*/platforms/` to exclude the platforms directory of Cordova projects
* `exclude			*/output/` + `*/output_web/` to exclude the output directory of any [Pelican](https://github.com/getpelican/pelican) project
* `exclude			*/bolt/` for [Bolt](https://bolt.cm/) projects
* `exclude			*/_site/` for Jekyll projects

## Automation

To automatically create backups I use crontab, since it is available on all platforms.

```bash
~ % crontab -e
```

and add:
```
# RSnapshot
0 */6 * * *    /usr/local/bin/rsnapshot -c /Users/martin/rsnapshot/rsnapshot.conf hourly
30 13 * * *    /usr/local/bin/rsnapshot -c /Users/martin/rsnapshot/rsnapshot.conf daily
0  11 * * 1    /usr/local/bin/rsnapshot -c /Users/martin/rsnapshot/rsnapshot.conf weekly
30 12 1 * *    /usr/local/bin/rsnapshot -c /Users/martin/rsnapshot/rsnapshot.conf monthly
```

Be sure to define some times when your PC typically is on. Or use other tools such as `anacron` or `launchd` to shedule the backups.

After some time there should be some directories `daily.0` to `daily.4` and so on in your `rsnapshot` directory.
