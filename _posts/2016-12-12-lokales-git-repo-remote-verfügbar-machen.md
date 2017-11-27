---
layout: post
title: Ein lokal vorhandenes git-Repository remote verfügbar machen
tags: git
---

Oft hat man kleine Projekte die man eigentlich nur lokal bearbeiten wollte und es ergibt sich, dass man doch auf einem anderen Rechner Zugriff darauf braucht.   
Natürlich hat man auch für die kleinsten lokalen Projekte Versionsverwaltung mit git bereits genutzt :-)

Wie bekommt man jetzt das lokale Repository auf einen Rechner im Internet, z.B. auf dem uberspace?

*   auf dem Server ein leeres Repository anlegen   
    `git init --bare PROJEKT.git`
*   lokal das remote Repository hinzufügen   
    `git remote add origin login@server:/PFAD.ZU.PROJEKT.git`
*   lokales Repository pushen   
    `git push --mirror origin`
*   auf dem zweiten Rechner dann einfach clonen mittels   
    `git clone login@server:/PFAD.ZU.PROJEKT.git`

---
__Update 2017:__
Inzwischen nutze ich gitea auf dem uberspace, also nicht mehr "raw" git per ssh,
an der prinzipiellen Vorgehensweise hat sich aber nichts geändert - nur die
Erstellung des leeren Repositories geht jetzt im Browser.
