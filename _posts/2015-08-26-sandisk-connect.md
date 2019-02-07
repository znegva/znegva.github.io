---
layout: post
title: SanDisk Connect zum laufen bringen
lang: de_DE
---

In Vorbereitung auf den diesjährigen Urlaub habe ich mich endlich mal   
ausführlicher mit der kleinen Festplatte/WiFi-Station SanDisk Connect   
auseinandergesetzt - im Folgenden nur noch _silberkiste_ genannt, das ist auch ihre SSID, aber dazu später mehr.   
Bisher kam sie einfach nur ab und zu mal per USB an den Rechner.

Um das auch einfach unter Debian bewerkstelligen zu können muss man erst mal   
`exfat-fuse` und `exfat-utils` installieren, dann kann man sie ganz normal mounten.

Jetzt zu den erdachten Anwendungsfällen im Urlaub:

*   einfache Backup-Möglichkeit für Fotos und Videos auf das alle Geräte (man macht ja heute mit zig verschiedenen Fotos) immer einfach Zugriff haben
*   Daten einfach zwischen allen Geräten austauschen zu können - dazu hat es sich als sinnvoll erwiesen sich im gleichen Netzwerk zu befinden. Genau das bietet mir die silberkiste, sie ist auch WLAN-Router
*   wenn doch mal Langeweile aufkommt ein paar Filme bereit zu haben

Also als erstes mal ein sicheres WLAN eingerichtet, per default bietet die silberkiste nur ein offenes WLAN an, WPA2 ist aber möglich und wird natürlich genutzt.   
So haben wir schonmal Punkt 2 erfüllt, alle Geräte können auch in fremden Landen in eingemeinsames Netzwerk gebracht werden.

Jetzt fehlt noch der einfache Zugriff... die SanDisk App mit Ihren unzähligen Berechtigungen wollte ich mir nicht auf das Telefon laden, es muss auch einfacher gehen.   
Und tatsächlich, man kommt per FTP auf die silberkiste drauf, ohne irgendetwas extra einrichten zu müssen - als Login wird das altbewährte `admin:admin` verwendet... naja, wir befinden uns ja im WPA2 gesicherten WLAN :).

Im Android habe ich mich sehr an den `ES File Explorer` gewöhnt, nicht gerade KISS, aber so muss man wenigstens nicht so lange nach einer passenden App suchen.   
Man kann also einfach eine FTP-Verbindung zur silberkiste einrichten und dann munter drauf los kopieren.
