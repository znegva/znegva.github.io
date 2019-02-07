---
layout: post
title: 'automatisches Backup von BackInTime unter Debian wieder richten'
lang: de_DE
tags: Debian backups
---

Irgendwann fiel mir auf, dass das automatische Backup von [Back in Time](http://backintime.le-web.org/) nicht mehr funktionierte o.O   
Die Logmeldungen (per default gehen die praktischerweise an `/dev/null`) ließen nichts Gutes erwarten:

    Back In Time  
    Version: 1.0.36

    Back In Time comes with ABSOLUTELY NO WARRANTY.  
    This is free software, and you are welcome to redistribute it  
    under certain conditions; type `backintime --license' for details.

    INFO: Lock  
    Traceback (most recent call last):  
      File "/usr/share/backintime/common/backintime.py", line 405, in <module>
        start_app()
      File "/usr/share/backintime/common/backintime.py", line 187, in start_app
        take_snapshot( cfg, False )
      File "/usr/share/backintime/common/backintime.py", line 56, in take_snapshot
        snapshots.Snapshots( cfg ).take_snapshot( force )
      File "/usr/share/backintime/common/snapshots.py", line 887, in take_snapshot
        hash_id = mount.Mount(cfg = self.config).mount()
      File "/usr/share/backintime/common/mount.py", line 51, in __init__
        if self.config.get_password_use_cache(self.profile_id):
      File "/usr/share/backintime/common/config.py", line 555, in get_password_use_cache
        default = not tools.check_home_encrypt()
      File "/usr/share/backintime/common/tools.py", line 470, in check_home_encrypt
        if not check_mountpoint(home):
      File "/usr/share/backintime/common/tools.py", line 462, in check_mountpoint
        subprocess.check_call(['mountpoint', path], stdout=open(os.devnull, 'w'))
      File "/usr/lib/python2.7/subprocess.py", line 535, in check_call
        retcode = call(*popenargs, **kwargs)
      File "/usr/lib/python2.7/subprocess.py", line 522, in call
        return Popen(*popenargs, **kwargs).wait()
      File "/usr/lib/python2.7/subprocess.py", line 710, in __init__
        errread, errwrite)
      File "/usr/lib/python2.7/subprocess.py", line 1335, in _execute_child
        raise child_exception
    OSError: [Errno 2] No such file or directory  

Das Komische war, dass ein manuell angestoßenes Backup problemlos funktionierte - bevor man jetzt den Fehler innerhalb des Backintime Codes sucht oder ein update installiert (wir sind unter Debian stable o.O) wird ein herum-arbeiten um den Fehler also vermutlich die schnellere Lösung sein.

 *   das automatische backup in BackInTime abgestellt, funktioniert ja sowieso nicht
 *   in Script geschrieben, in dem alle Pfade nochmals exportiert werden und mit Rückmeldung an mich (und nicht an `/dev/null` wie beim von BackInTime angelegten Crontab-Eintrag)
```
#!/bin/bash
#title          :runbakintime
#description    :This script will fire a backup job in backintime, announces correct PATH to be called even from cron
#author         :martin
#version        :20161125
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/home/martin/scripts  
/usr/bin/nice -n 19 /usr/bin/backintime --backup-job 2>&1
```
 *   `crontab -e` und das eigene Script alle 6 Stunden aufrufen lassen
```
0 */6 * * * /home/martin/scripts/runbackintime
```
 * Fertig
