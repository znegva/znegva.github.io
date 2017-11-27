---
layout: post
title: Bauknecht WA Care 32 Di reparieren
lang: de_DE
tags: DIY, Hardware
---

Unsere Waschmaschine hat den Geist aufgegeben :(   
Gerade noch hat sie fleißig gewaschen, im nächsten Moment sind alle Lämpchen aus und man muss die klatschnasse Wäsche von Hand auswringen.

Eine kurze Recherche hat ergeben, dass oftmals eine Gruppe von Bauteilen den Geist aufgibt weil sie etwas zu klein dimensioniert sind - [Geplante Obsoleszenz](https://de.wikipedia.org/wiki/Geplante_Obsoleszenz) ick hör dir trapsen.   
Schnell die Maschine zerlegt, und tatsächlich sehen die üblichen Verdächtigen (Widerstand R020, Spule L003 und der IC LNK304PN) sehen komisch aus:

![](/assets/bauknecht_L003.jpg)

![](/assets/bauknecht_LNK304PN.jpg)Die Spule ist geplatzt, und auf dem IC ist auch zu erkennen dass er wohl etwas zu viel Strom abbekommen hat.

Also Glück im Unglück - das Problem haben wohl schon viele vor mir gelöst, dann kann ich das auch.

### Bauteile bestellen

Da gibt es fertige Sets von verschiedensten Anbietern, einfach nach LNK304PN suchen.   
Ich habe ein Set (22 Ohm Widerstand, 470µH Spule, IC LNK304PN inkl. Sockel und etwas Entlötlitze) für €7,50 bei ebay bestellt, die Teile separat bei Conrad wären inkl. Versandkosten genauso teuer gewesen.

### Kaputte Verdächtige auslöten

Dazu muss man ordentlich Lötzinn auf die Stellen geben damit der Industriezinn weich wird und die Versiegelung auf der Rückseite verdampfen kann (Fenster öffnen!).   
Von der Rückseite der das Zinn weich machen und Bauteile nach vorne hin mit einer Zange wegziehen.   
Da der IC an 7 Stellen verlötet ist bekommt man ihn schwer ab weil man nicht alle Stellen gleichzeitig weich bekommt. Einfach die Füßchen möglichst weit oben abknipsen und dann einzeln auslöten.   
Am Ende müssen noch die Löcher komplett frei gemacht werden, damit man die neuen Bauteile auch durchstecken kann - ich mir dazu noch eine Entlötpumpe gekauft, mit Entlötlitze wollte es einfach nicht so richtig klappen.

### Neue Teile einlöten

Mit dem entlöten und freimachen war der aufwändigste Teil erledigt, die neuen Teile einzulöten ging schnell von der Hand.

### Fertig

Alles wieder zusammengebaut und die Stecker an die richtige Stelle der Platine gesteckt (vorher Fotos machen). Alles läuft wieder :-)

![](/assets/bauknecht_fertig.jpg)
