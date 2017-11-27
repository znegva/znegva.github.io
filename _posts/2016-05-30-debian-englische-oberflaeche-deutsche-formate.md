---
layout: post
title: 'Debian: englische Oberfläche, deutsche Formate'
lang: de_DE
tags: Debian
---

Wenn man gerne sein Debian auf englisch nutzen möchte aber gleichzeitig den Rest (z.B. Datums- und Papierformate) nach den Standards seines Landes (in meinem Fall de_DE) eingestellt haben möchte, dann geht das folgendermaßen:

in `/etc/default/locale`:

    LANG="de_DE.UTF-8"  
    LC_MESSAGES="en_US.UTF.8"  

Nach einem Neustart kann man dann prüfen ob alle sgeklapt hat:

    ~ % locale
    LANG="de_DE.UTF-8"  
    LANGUAGE=  
    LC_CTYPE="de_DE.UTF-8"  
    LC_NUMERIC="de_DE.UTF-8"  
    LC_TIME="de_DE.UTF-8"  
    LC_COLLATE="de_DE.UTF-8"  
    LC_MONETARY="de_DE.UTF-8"  
    LC_MESSAGES="en_US.UTF.8"  
    LC_PAPER="de_DE.UTF-8"  
    LC_NAME="de_DE.UTF-8"  
    LC_ADDRESS="de_DE.UTF-8"  
    LC_TELEPHONE="de_DE.UTF-8"  
    LC_MEASUREMENT="de_DE.UTF-8"  
    LC_IDENTIFICATION="de_DE.UTF-8"  
    LC_ALL=  

Wenn man z.B. sein Papierformat (`LC_PAPER`) gerne anders mag, der kann natürlich auch eine entsprechende Sonderregel in `/etc/default/locale` hinterlegen.
