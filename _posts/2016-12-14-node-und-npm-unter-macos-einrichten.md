---
layout: post
title: node und npm unter macOS einrichten
lang: de_DE
tags: macOS node
---

Viele aktuelle Projekte wie z.B. ionic oder Ghost bauen auf [node.js](https://nodejs.org/) auf, dessen Paketmanager ist wiederum [npm](https://www.npmjs.com/).

Prinzipiell wäre alles ganz einfach mit ein paar Befehlen an [brew](http://brew.sh/) abgehakt, wir möchten jedoch `nmp`-Pakete global auch ohne root-Rechte installieren lassen, daher sind ein paar Vorbereitungen notwendig (Anleitung angelehnt an [diesen Ablauf](https://johnpapa.net/how-to-use-npm-global-without-sudo-on-osx/).

Wir müssen npm mitteilen wo es seine Pakete installieren soll (nämlich nach `~/.npm-packages`), und wir müssen unserer shell (hier beispielhaft die zsh) sagen wo diese Pakete liegen.

``` sh
~ % mkdir "${HOME}/.npm-packages"
~ % echo NPM_PACKAGES="${HOME}/.npm-packages" >> ${HOME}/.zshrc
~ % echo prefix=${HOME}/.npm-packages >> ${HOME}/.npmrc
~ % echo PATH=\"\$NPM_PACKAGES/bin\:\$PATH\" >> ${HOME}/.zshrc
~ % brew install npm
```

Das wars auch schon, zusammen mit npm wird auch gleich node installiert.   
Zeit für einen ersten Test
 ``` sh
~ % npm install bower -g
~ % type -a bower
bower is /Users/martin/.npm-packages/bin/bower  
```

So soll es sein!

## verschiedene node-Versionen mittels brew verwalten

Gelegentlich kann es vorkommen, dass eine bestimmte (ältere) Version von node.js benötigt wird.   
Entweder kann man dafür [nvm](https://github.com/creationix/nvm) (Node Version Manager) nehmen - oder man macht es einfach mittels brew.

Welche Versionen sind verfügbar?

``` sh
~ % brew search node
leafnode    node ✔      node@0.10   node@4      node@6 ✔    nodeenv  
llnode      node-build  node@0.12   node@5      nodebrew    nodenv  
Caskroom/cask/mindnode-pro            Caskroom/cask/nodeclipse  
Caskroom/cask/node-profiler           Caskroom/cask/printnode  
Caskroom/cask/nodebox                 Caskroom/cask/soundnode  
~ %
```

Aha, also 0.10, 0.12, 4, 5, 6 und die aktuellste 7.

Um eine andere als die aktuell (`node` aka 7.2.1) genutzte Version installieren zu können muss die aktuelle mittels `brew unlink node` "entlinkt" werden, anschließend kann die ältere mittels `brew install node@6`installiert werden und wird sogleich zur aktuell genutzten gelinkt.

Beide Versionen sind jetzt parallel installiert, direkt genutzt werden kann immer nur die aktuell gelinkte.

``` sh
~ % node -v
v6.9.2  
~ % brew unlink node@6
Unlinking /usr/local/Cellar/node@6/6.9.2... 7 symlinks removed  
~ % brew link node
Linking /usr/local/Cellar/node/7.2.1... 7 symlinks created  
~ % node -v
v7.2.1  
~ %
```
