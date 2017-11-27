---
layout: post
title: macOS einrichten für Umsteiger
tags: macOS, aaargh
---

Als neues Mitglied in der Apfel-Familie hier ein paar Eindrücke und durchgeführte Schritte.

## los gehts

Es handelt sich um einen gebrauchten Mac Mini von 2012, direkt nach dem auspacken und anschließen war erstmal eine Registrierung zur Apple-ID notwendig.   
Praktischerweise habe ich keine Apple-Tastatur und bereits die Eingabe des @-Zeichens gestaltete sich schwierig (`Alt + L` ist der Trick) - weiteres zur Tastatureinrichtung unten.

Als erstes war ein Update auf macOS 10.12 Sierra fällig - nach ein paar Stunden Download war das erledigt und die Installation klappte auch problemlos.   
Direkt nach dem Download und noch vor dem eigentlichen Update habe ich die heruntergeladene _App_ aus dem `Applications` Verzeichnis kopiert, man kann diese Kopie verwenden um einen bootfähigen USB-Stick für spätere Neuinstallationen zu erzeugen.

## Tastatur

Offenbar halten es die Apple-Jungs (und Mädels) nicht für nötig im Jahr 2016 auch andere als die von ihnen selbst verkauften Tastaturen vernünftig von Haus aus zu unterstützen. Die Einrichtung ist möglich, aber von der viel gelobten Benutzerfreundlichkeit eines Apple-Rechners habe ich nicht viel bemerkt.

Abhilfe schaffte [folgende Anleitung](https://www.administrator.de/wissen/deutsche-pc-tastatur-apple-mac-einrichten-221078.html):

*   die dort erhältliche `pc_tastatur.keylayout` in `~/Library/Keyboard Layouts` kopiert
*   in den Systemeinstellungen unter Keyboard/Tastatur eine neue Eingabemethode hinzufügen
*   im Menü oben neben der Uhr "PC Tastatur Deustch" auswählen
*   danach ging es in manchen Programmen, in anderen nicht - ich glaube ein Neustart hatte geholfen o.O
*   `Alt` und `MOD` (aka Windowstaste) können vertauscht sein, das kann man in den Tastatureinstellungen unter `Modifier Keys...` tauschen
*   überhaupt sollte man sich an die neuen Namen gewöhnen   
    `Win` heißt `Command key`   
    `Alt` heißt `Option`   
    `Ctrl` heißt `Control` (na immerhin)
*   auch andere gewohnte Tasten bzw. Kombinationen funktionieren anders (zumindest innerhalb der Mac Programme, z.B. im Browser kann das schon wieder anders sein):
    *   normal bewegt man sich mit `Ctrl + Links/Rechts`Wortweise durch einen Text, im macOS ist es `Alt + Links/Rechts`
    *   `Pos1` und `End` funktionieren nicht (immer) um an den Anfang bzw Ende einer Zeile zu springen, stattdessen nimmt man `Win + Links/Rechts`
    *   statt `Ctrl+C` und `Ctrl+V` nimmt man `Win+C` und `Win+V`
    *   besonders nervig ist, dass die `Del` Taste zum löschen nicht funktioniert, man muss im Finder die Fingerübung `Win + Backspace` vollbringen

## brew

Ein sehr hilfreiches und für die nächsten Schritte notwendiges Werkzeug ist [Homebrew - The missing package manager for macOS](http://brew.sh/)!   
Es lassen sich wie unter Linux/Debian mit apt gewohnt einfach und schnell Programme installieren. Für grafische Programme kommt die Erweiterung _Cask_ zum Einsatz.

### Installation

denkbar einfach mittels folgendem Einzeiler im Terminal

    ~ % /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Bei der Installation wird sich ganz automatisch um alle Abhängigkeiten gekümmert.

### Verwendung

Es gibt eine Menge [Möglichkeiten](https://github.com/Homebrew/brew/blob/master/docs/FAQ.md) der Verwendung, _learning by doing_ - wir beginnen damit alles was man so denkt gebrauchen zu können zu installieren:

    ~ % brew install htop multitail wget irssi ack tmux ranger mpv
    ~ % brew cask install google-chrome
    ~ % brew cask install libreoffice
    ~ % brew cask install thunderbird
    ~ % brew cask install mplayerx

Die Einrichtung von Thunderbird erfolgt dann [wie beschrieben]({% post_url 2016-06-06-kalender-kontakte-todos-auf-uberspace %}).

Pakete suchen kann man mit `~ % brew search XYZ` bzw. `~ % brew cask search XYZ`.

## besseres Terminal mit zsh

Mittels brew läßt sich auch schnell und einfach eine bessere Terminal-App ([iTerm2](https://www.iterm2.com/)) und eine aktuelle Version der `zsh` installieren und gleich als default-shell einrichten.   
Für den Komfort gleich `zsh-completions` hinterher, notwendige Pfadanpassungen in der `.zshrc` dazu und schlecht erstellte Verzeichnisse [geradegerückt](http://stackoverflow.com/a/22753363):

    ~ % brew cask install iterm2
    ~ % brew install zsh zsh-completions
    ~ % sudo dscl . -create /Users/$USER UserShell /usr/local/bin/zsh
    ~ % echo "fpath=(/usr/local/share/zsh-completions $fpath)" >> ~/.zshrc
    ~ % compaudit | xargs chmod g-w

Für einen beeindruckenden fuzzy-file-finder sollte man sich mal `fzf` anschauen, natürlich auch mittels brew installierbar.

## Tiling Window Manager

Im Debian habe ich den Awesome Window Manager verwendet, etwas vergleichbares gibt es leider für macOS nicht.   
Zunächst hatte ich [Amethyst](https://github.com/ianyh/Amethyst) versucht, so richtig warm wurde ich damit aber nicht, irgendwie machte der nicht was er sollte.   
Der nächste Versuch mit [Spectacle](https://www.spectacleapp.com/) war schon besser - kein automatisches tiling, aber einfache Vorgaben mit denen man seine Fenster schnell ausrichten kann. Es stehen diverse Modi zur Verfügung, von mir aktiv genutzt wird:

*   Linke/Rechte/Obere/Untere Hälfte bzw. Drittel bzw. 2Drittel
*   Linkes/Rechtes Oberes/Unteres Viertel

## Syncthing

Wie [hier beschrieben]({% post_url 2016-06-06-kalender-kontakte-todos-auf-uberspace %}) nutze ich [Syncthing](https://syncthing.net/) zum Abgleich diverser Daten zwischen
verschiedenen Geräten (leider kein iOS Client), auch im macOS ist es mit `brew` schnell installiert und als Service eingerichtet:

    ~ % brew install syncthing
    ~ % brew services start syncthing
    ~ % syncthing -browser-only

## Drucker

Hier mal ein großes Lob, die Druckereinrichtung war denkbar einfach, der EPSON WorkForce Netzwerkdrucker wurde direkt im Netzwerk gefunden und funktionierte auf Anhieb. Sogar scannen direkt vom Rechner aus funktioniert.

## macOS "internes" verbessern

An so einigen Stellen waren Anpassungen an den Standardprogrammen von macOS notwendig um sie benutzbar zu machen

Im **Finder** mittels `Win+Shift+G` die freie Auswahl für das anzuzeigende Verzeichnis öffnen und ins home-Verzeichenis `~` wechseln und dann dieses gleich mal in die Seitenleiste ziehen.

Im **Chrome** `F5` zum reload der Seite einrichten:   
In den Tastatureinstellungen einen neuen Shortcut für die Anwendung _Google Chrome_ einrichten - dort muss man ernsthaft die genaue Bezeichnung des Menüpunktes eintragen (wer denkt sich soetwas aus??).   
`F5` wurde mit `Reload This Page` (cAsE sensitive!) und `F12` mit `Developer Tools` verknüpft.

Als **Standardmediaplayer** etwas anderes als Quicktime einzurichten wird einem auch unnötig schwer gemacht:   
`Ctrl + Klick` auf die zu öffnende Datei --> `Get Info` --> `Open with` --> App auswählen (z.B. MPlayerX) --> `Change All` klicken --> Bestätigen

Die **Spotlight** App ist auch nicht so doll wie sie beschrieben wird, man kann sie aber ersetzen oder erweitern.   
_Möglichkeit 1_ ist [Alfred](https://www.alfredapp.com/) als Alternative - kurz ausprobiert und schnell gemerkt dass die wirklich brauchbaren Sachen nur mit den kostenpflichtigen [Powerpack](https://www.alfredapp.com/powerpack/)möglich sind.

_Möglichkeit 2_ war/ist [flashlight](https://github.com/w0lfschild/Flashlight), eine Art Erweiterung von spotlight mit vielen Plugins.   
Die Installation gestaltet sich etwas umständlich, da offenbar tief in die Innereien des macOS eingegriffen werden muss:

*   macOS neu booten
*   während des Bootvorganges mittels `Win+R` in den "Recovery Modus" wechseln
*   dort im Terminal den Befehl `crsutil disable`eingeben um die "System Integrity Protection" zu deaktivieren
*   normal neu starten und `mySIMBL` installieren (mittels brew) und ausführen und die Installation abschließen
*   `flashlight` mittels brew installieren
*   wieder im "Recovery Modus" die "System Integrity Protection" mittels `crsutil enable` aktivieren

Am Ende bleibt man dann wohl doch bei Spotlight und hofft auf Besserung.

Alles sehr umständlich, und irgendwie ist das Ganze auch eher instabil, ob ich bei der Installation etwas falsch gemacht habe oder ob es einfach so ist kann ich leider nicht sagen :(

Wer sich an alte _TuneUp Utilities_ Zeiten unter Windows XP zurückerinnern möchten kann sich [OnyX](http://www.titanium.free.fr/) installieren:

    ~ % brew cask install onyx
