---
layout: post
title: Encrypted backups with rsnapshot and GPG
tags: macOS Debian backups GPG rsync brew
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

As you can see my rsnapshot directory is `/Users/martin/rsnapshot/`, the `rsnapshot.conf` shown above is saved in this directory.

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

Be sure to define some times when your PC typically is on. Or use other tools such as `anacron` or `launchd` to schedule the backups.

After some time there should be some directories `daily.0` to `daily.4` and so on in your `rsnapshot` directory.

## Encryption

Only a distributed backup is a good backup, so lets encrypt our files so we can put them anywhere without being scared of thieves.

I use GPG with AES256 for encryption, if it is not already installed use the package manager of your choice to do so.

I got into the habit of creating a backup every week or so, encrypt it, and put it to some external hard drives. To automate the generation I build a little script:

```python
#! /usr/bin/env python

import subprocess
import os
import datetime
import platform

rsnapshotDir = "/Users/martin/rsnapshot/"
deleteTarWhenFinished = True
debug = False

def print_green(t):
	print('\033[92m' + t + '\033[0m')

def call(command):
	if debug:
		print command
	else:
		subprocess.call(command, shell=True)

def createBackup():
	print_green("\n### Creating new backup with rsnapshot \n")
	command = "rsnapshot -c " + rsnapshotDir + "rsnapshot.conf hourly"
	call(command)

def createAndEncryptTar():
	print_green("\n### Creating tar-file from latest backup \n")
	date = datetime.datetime.now().strftime("%Y%m%d")
	os = platform.system()
	latest = "hourly.0"
	tarname = date + ".backup." + os + ".tar"
	command = 'tar pcf ' + tarname + ' ' + latest
	call(command)

	print_green("\n### Encrypt tar-file \n")
	command = "gpg -c --cipher-algo AES256 " + tarname
	call(command)

	if deleteTarWhenFinished:
		print_green("\n### Deleting tar-file \n")
		command = "rm " + tarname
		call(command)

createBackup()
createAndEncryptTar()
```

At the end GPG asks for a password, be sure to use a good one that you can remember when shtf :).
