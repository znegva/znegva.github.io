---
layout: post
title: Mail backups with local Dovecot
tags: backups
---

For years I struggeled with the right way of backing up my emails  - operating systems, devices and mail-programs changed from time to time...

Maybe now I found the best way of doing it right: Use a locally installed [Dovecot](https://www.dovecot.org/) IMAP-Server storing to [Maildir](https://wiki2.dovecot.org/MailboxFormat/Maildir).

Using a server set up only for this purpose allows me do use any frontend to manage the mails (such as Thunderbird, or whatever you like) and at the same time any backup solution (rsnapshot, TimeMachine, BackInTime).  
Dovecot can be set up on any UNIX-like OS, so probably I can set it up in the future to and just use the backup of my _Maildir_.


Here is how I did it on macOS (please also see [here](https://xdeb.org/post/2014/03/07/running-dovecot-as-a-local-only-imap-server-on-os-x/)), but should be nearly the same on any other OS.

__install:__
```bash
~ % brew install dovecot
```

__adopt config-files__
```
~ % cp -pr /usr/local/Cellar/dovecot/2.3.2.1_1/share/doc/dovecot/example-config/ /usr/local/etc/dovecot/
```

create `/use/local/etc/dovecot/local.conf` (shortended a bit, please see [post on xdeb.org for a commented example](https://xdeb.org/post/2014/03/07/running-dovecot-as-a-local-only-imap-server-on-os-x/)):
```conf
# A comma separated list of IPs or hosts where to listen in for connections. 
# "*" listens in all IPv4 interfaces, "::" listens in all IPv6 interfaces.
listen = 127.0.0.1

# Protocols we want to be serving.
protocols = imap

# Static passdb.

# This can be used for situations where Dovecot doesn't need to verify the
# username or the password, or if there is a single password for all users:
passdb {
  driver = static
  args = password=VERY_SECRET_PASSWORD
}

# Location for users' mailboxes. 
#
# There are a few special variables you can use, eg.:
#
#   %u - username
#   %n - user part in user@domain, same as %u if there's no domain
#   %d - domain part in user@domain, empty if there's no domain
#   %h - home directory
#
# See doc/wiki/Variables.txt for full list. Some examples:
mail_location = maildir:/Users/martin/%n


# System user and group used to access mails. If you use multiple, userdb
# can override these by returning uid or gid fields. You can use either numbers
# or names. <doc/wiki/UserIds.txt>
mail_uid = martin
mail_gid = admin

# SSL/TLS support: yes, no, required. <doc/wiki/SSL.txt>
ssl = no

# Login user is internally used by login processes. This is the most untrusted
# user in Dovecot system. It shouldn't have access to anything at all.
default_login_user = _dovenull

# Internal user is used by unprivileged processes. It should be separate from
# login user, so that login processes can't disturb other processes.
default_internal_user = _dovecot
# Internal group is expected to be the primary group of the default_internal_user.
default_internal_group = mail

# Setting limits.
default_process_limit = 10
default_client_limit = 50
```

disable/comment default auth-settings in `/usr/local/etc/dovecot/conf.d/10-auth.conf`
```conf
#!include auth-system.conf.ext
```

disable/comment default ssl-settings in `/usr/local/etc/dovecot/conf.d/10-ssl.conf`
```conf
#ssl_cert = </etc/ssl/certs/dovecot.pem
#ssl_key = </etc/ssl/private/dovecot.pem
```

__run the dovecot-server__ (it has to be done as root...)
```bash
~ % sudo brew service start dovecot
```

You can stop the serve with:
```bash
~ % sudo brew service stop dovecot
```

__manage your mails__

Now you can configure your email-client of choice to point to an IMAP-account at `127.0.0.1` using any username and the password you set in `local.conf`.

__Backup__

The mails should appear inside `~/Maildir` and are ready to be backed up.  
Any directories created inside of the IMAP-account (using your mail-client) are stored as hidden directories (leading dot), so be sure to backup them too.