---
layout: post
title: aktuelle rsync Version auf macOS
lang: de_DE
tags: aaargh brew macOS rsync
---

macOS Sierra kommt leider mit einer **sehr** alten Version von rsync:

``` sh
~ % rsync --version                                      
rsync  version 2.6.9  protocol version 29  
Copyright (C) 1996-2006 by Andrew Tridgell, Wayne Davison, and others.  
<http://rsync.samba.org/>  
Capabilities: 64-bit files, socketpairs, hard links, symlinks, batchfiles,  
              inplace, IPv6, 64-bit system inums, 64-bit internal inums

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you  
are welcome to redistribute it under certain conditions.  See the GNU  
General Public Licence for details.  
~ %
```

Wie so oft ist `brew` die Lösung - es gibt sogar ein eigenes Paket für Formulas die bereits vorhandene Software in macOS ersetzen: [Homebrew Dupes](https://github.com/Homebrew/homebrew-dupes).

Die kann man auch direkt mittels `tap` aktivieren und dann rsync installieren:

``` sh
~ % brew tap homebrew/dupes
~ % brew install rsync
```

Nachdem alle Abhängigkeiten installiert und `rsync` kompiliert ist liegt es in einer aktuellen Version in `/usr/local/bin/` vor.

Wenn man dieses standardmäßig nutzen möchte ist noch eine Anpassung in `/private/etc/paths` vornehmen, und zwar `/usr/local/bin` vor `/usr/bin` sortieren.   
Alternativ kann man natürlich auch die neue Version direkt als `usr/local/bin/rsync` aufrufen bzw. angeben.

``` sh
~ % which rsync
/usr/local/bin/rsync
~ % rsync --version
rsync  version 3.1.2  protocol version 31  
Copyright (C) 1996-2015 by Andrew Tridgell, Wayne Davison, and others.  
Web site: http://rsync.samba.org/  
Capabilities:  
    64-bit files, 64-bit inums, 64-bit timestamps, 64-bit long ints,
    socketpairs, hardlinks, symlinks, IPv6, batchfiles, inplace,
    append, ACLs, xattrs, iconv, symtimes, no prealloc, file-flags

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you are welcome to redistribute it under certain conditions.  See the GNU  
General Public Licence for details.  
~ %
```

Mit dieser Version laufen dann auch geliebte Programme wie `grsync` :)
