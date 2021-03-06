---
layout: post
title: mail für lokale Nachrichten einrichten nachdem man den hostnamen geändert hat
lang: de_DE
tags: Debian
---

## Situation

*   bei der Installation hat man einen hostnamen (z.B. `lamebox`) für sein Debian angegeben
*   und sich später dazu entschieden diesen zu ändern indem man in `/etc/hostname` z.B. `coolbox` einträgt
*   später möchte man mittels `mail` lokal Nachrichten von Nutzer zu Nutzer senden (z.B. ein Script erstattet Bericht)
*   dabei stellt man fest, dass als Domain/Hostname für die Nutzer noch der alte Hostname genutzt wird

```
Message 1:  
From martin@lamebox Sat Mar 19 19:32:28 2016  
Envelope-to: martin@lamebox  
Delivery-date: Sat, 19 Mar 2016 19:32:28 +0100  
To: martin@lamebox  
Subject: my message  
```

## Lösung

zunächst einmal muss man rausfinden woher `mail` den hostnamen holt, es stellt sich heraus, dass es im Debian (mal wieder) einen Sonderweg gibt, und zwar kommt es aus `/etc/mailname` - hier findet man noch immer `lamebox`.

Also schnell an den aktuellen hostname angepasst, und exim4 (der mailserver der hinter `mail` steht) neugestartet.

Dann mal fix eine Nachricht versandt:

    % mail martin
    [...]
    % mail
    No mail for martin  

Da ging wohl etwas schief.   
Also mal direkt testen, mittels einer Nachricht in Exims `address testing mode`:

    root@coolbox:/home/martin# exim -bt martin  
    R: nonlocal for martin@coolbox  
    martin@coolbox is undeliverable: Mailing to remote domains not supported  

Aha, er denkt also `coolbox` wäre eine Remote-Adresse... woher holt er denn die Infos was Remote und was Lokal ist?

    root@coolbox:/home/martin# ls -lha /var/lib/exim4/  
    total 36K  
    drwxr-xr-x  2 root root        4.0K Mar 19 19:04 .  
    drwxr-xr-x 69 root root        4.0K Mar 10 13:14 ..  
    -rw-r--r--  1 root root           4 Apr 29  2015 berkeleydbvers.txt
    -rw-r--r--  1 root Debian-exim  24K Mar 19 19:04 config.autogenerated
    root@coolbox:/home/martin# less /var/lib/exim4/config.autogenerated |grep box  
    MAIN_LOCAL_DOMAINS=@:localhost:lamebox  
    ETC_MAILNAME=coolbox  

OK, das steht offenbar in `MAIN_LOCAL_DOMAINS` - da das config-file schon das schöne `autogenerated` im Namen hat sollte ich daran direkt wohl nicht herum-editieren.

Also die harte Tour, `dpgk-reconfigure` und den Service neu starten:

    root@coolbox:/etc/exim4# dpkg-reconfigure exim4-config  
    root@coolbox:/etc/exim4# systemctl restart exim4.service  

Danach konnte ich mir endlich selber Nachrichten schicken!
