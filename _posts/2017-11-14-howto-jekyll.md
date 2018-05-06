---
layout: post
title: Jekyll lokal einrichten
lang: de_DE
tags: macOS Debian Jekyll snippets
update: 2018-05-06
---

Mit [github-Pages](https://pages.github.com/) gibt es eine einfache Möglichkeit
eine statische Webseite zu veröffentlichen. Dabei wird auf den Jekyll als
Static Site Generator gesetzt.

Um die erstellten Seiten vor dem commiten kurz prüfen zu können bietet es sich an
Jekyll lokal zu installieren.  
Grundlage ist in jedem Fall Ruby zu installieren, da Jekyll darauf aufbaut.

## macOS

Unter macOS bekommt man mit `brew` eine relativ aktuelle Ruby Version
``` zsh
~ % brew install ruby
~ % ruby -v
ruby 2.4.2p198 (2017-09-14 revision 59899) [x86_64-darwin16]
```

Anschließend kann man sich mit dem Ruby-eigenen Peketmanager `gem` die nötigen
Ruby-Pakete in aktueller Version installieren:
``` zsh
~ % gem install bundler jekyll
```

`bundler` wird gebraucht um evtl. notwendige Pakete (definiert im `Gemfile`
des Projektes) automatisch zu installieren/nutzen.  
`jekyll` ist ja unser eigentliches Ziel :).



## Debian

Auch unter Debian nutzen wir wieder den Hauseigenen Paketmanager um zunächst
die Ruby-Pakete zu installieren.  
Hierbei müssen wir beachten dass wir das `ruby-dev` Paket als auch die
`build-essentials` installieren, da ansonsten [nokogiri](http://www.nokogiri.org/tutorials/installing_nokogiri.html) als auch
[ffi]() Probleme bereiten werden:

``` zsh
~ % sudo aptitude install build-essential ruby-dev
```

Anschließend nutzen wir wieder `gem` um aktuelle Versionen von `bundler` und `jekyll`
zu bekommen - in den Debian Repositories ist Jekyll auch zu finden, jedoch in einer
sehr alten Version!
``` zsh
~ % sudo gem install bundler jekyll
```

## Testlauf

Wenn alles installiert ist kann ein kurzer Testlauf gestartet werden.
``` zsh
~ % git clone "https://github.com/pages-themes/minimal.git"
~ % cd minimal
~/minimal % bundle exec jekyll serve
```

Die Seite sollte jetzt unter `http://127.0.0.1:4000` aufrufbar sein.

## Testen im Netzwerk

Per default wird die Webseite nur von aus 127.0.0.1 geliefert, etwas unpraktisch
wenn man die Seite auch mit anderen Geräten im Netzwerk (z.B. Smartphones) testen will.  
Mann kann aber explizit die IP-Adresse bzw. host mitgeben.

``` sh
~/minimal % bundle exec jekyll serve --host=0.0.0.0
```

Jetzt ist die Webseite auch von allen anderen Computern im Netzwerk erreichbar,
über die IP-Adresse des Rechners... einzig {% raw %}`{{ site.url }}`{% endraw %} in
den templates/layouts linkt noch auf `0.0.0.0`.

Auch das lässt sich beheben, indem man direkt die korrekte IP-Adresse mit übergibt:  

__macOS:__  
``` sh
~/minimal % bundle exec jekyll serve --host=$(ipconfig getifaddr en0)
```

__Debian:__  
``` sh
~/minimal % bundle exec jekyll serve --host=$(hostname -I | cut -d' ' -f1)
```
