---
layout: post
title: Bolt CMS aufsetzen
lang: de_DE
tags: BoltCMS
---

## Installation

Auf dem lokalen Rechner hat die normale command-line Installation aus der [Bolt Doku](https://docs.bolt.cm/3.2/installation/install-command-line) problemlos geklappt:

``` sh
~ % mkdir mybolt && cd mybolt
~/mybolt % curl -O https://bolt.cm/distribution/bolt-latest.tar.gz
~/mybolt % tar -xzf bolt-latest.tar.gz --strip-components=1
~/mybolt % php app/nut setup:sync
```

Danach falls nötig noch einige Rechte setzen (unter macOS ist der Apache in der Gruppe `_www`)

``` sh
~/mybolt % sudo chgrp -R _www ../mybolt
~/mybolt % chmod -R g+rwx app/cache/ app/config/ app/database/ extensions/
~/mybolt % chmod -R g+rwx public/thumbs/ public/extensions/ public/files/ public/theme/
```

Als Datenbank möchte ich SQLite benutzen, somit ist keine Anpassung notwendig - andernfalls bitte in der [Bolt Doku](https://docs.bolt.cm/3.2/configuration/database) nachsehen.

Als nächstes dann noch fix ein Softlink von `~/Sites` aus legen:
``` sh
~/mybolt % ln -s ~/mybolt/public/ ~/Sites/mybolt
```
Jetzt kann man man versuchen unter _localhost/~martin/mybolt/_ zu prüfen ob alles geklappt hat. Bei mir lief es von Anfang an.   
Bolt fordert auf einen ersten Benutzer anzulegen, dem kommt man natürlich nach und schon kann es losgehen.

## Theme erstellen

Als erstes möchte man sicher ein eigenes Theme für seine neue Seite erstellen, dazu könnte man z.B. das default-Theme als Vorlage benutzen:

``` sh
~/mybolt % cd public/theme
theme % cp -r base-2016 mytheme  
```
Damit das neue Theme verwendet wird muss diese noch in der Datei `app/config/config.yml` beim Punkt `theme` angepasst werden. Dazu kann man entweder die Datei im Editor öffnen und bearbeiten, oder man macht es direkt über das Bolt-Backend im Browser unter dem Menüpunkt `Settings -> Configuration -> Main Configuration`.

## git

Bevor man jetzt anfängt das Theme zu verändern sollte man ein git repository einrichten um Änderungen festhalten zu können. Es war bereits eine `.gitignore`dabei, diese habe ich noch ein wenig erweitert, z.B. braucht das default-Theme nicht getrackt zu werden:

```
# we dont need the default theme
public/theme/base-2016/  
```

Jetzt sollte alles soweit eingerichtet sein dass man anfangen kann sein Theme und auch die [Content Types](https://docs.bolt.cm/3.2/contenttypes/intro)anzupassen.

Der eigentliche Inhalt der Webseite (also Texte, Bilder usw.) wird in der Datenbank bzw. dem `files` Subfolder gespeichert und in der aktuellen Konfiguration nicht mittels git getrackt.   
Um deren Sicherung muss man sich also anderweitig kümmern!
