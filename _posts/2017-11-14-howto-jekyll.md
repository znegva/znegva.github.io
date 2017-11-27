---
layout: post
title: Jekyll lokal einrichten
lang: de_DE
tags: macOS Debian Jekyll
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



## Debian 8

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
