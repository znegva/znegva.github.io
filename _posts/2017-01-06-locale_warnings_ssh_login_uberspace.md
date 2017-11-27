---
layout: post
title: locale Warnungen nach ssh-Login verhindern
lang: de_DE
tags: uberspace, macOS, ssh
---

Beim SSH-Login auf dem uberspace versuchen die macOS Terminal App als auch iTerm2 ihre locale-Einstellungen mitzusenden. Leider versteht der uberspace-Server (CentOS) das nicht und es kommen einem eine handvoll Fehlermeldungen entgegen:

    perl: warning: Setting locale failed.  
    perl: warning: Please check that your locale settings:  
    LANGUAGE = (unset),  
    LC_ALL = (unset),  
    LC_CTYPE = "UTF-8",  
    LANG = (unset)  
        are supported and installed on your system.
    perl: warning: Falling back to the standard locale ("C").  

Da es sowieso nicht verstanden wird kann man das senden der locale-Einstellungen also auch gleich abstellen.

Entweder global in der `/etc/shh/ssh_config` indem man die Zeile `SendEnv LANG LC_*` auskommentiert.

Oder als User in der Terminal App mittels:   
`Preferences > Settings > Advanced > Set locale environment variables on startup`

bzw. in iTerm2 mittels:   
`Preferences > Profiles > Terminal > Environment > Set locale variables automatically`
