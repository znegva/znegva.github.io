---
layout: post
title: 'Kalender, Kontakte und ToDos lokal und auf dem Android Gerät, ohne Google'
tags: uberspace
---

---
__Update 2017:__
Inzwischen bin ich von der hier beschriebenen Lösung für Kalender und
Kontakte auf [Nextcloud](https://nextcloud.com/) umgestiegen, für ToDos nutze ich Kalender-Tasks und [gitea](https://gitea.io/)-Issues.
Natürlich noch immer alles auf dem [uberspace](https://uberspace.de/).
<!-- TODO: create posts and link to them -->

---

Entweder man überläßt Google seine Kalender, Kontakte und ToDos und kann einfach per Android-Telefon bzw. Weboberfläche darauf zugreifen .... oder man wählt den aufwändigen Weg und versucht alles in die eigene Hand (bzw. die Hand des Serveranbieters deiner Wahl) zu nehmen.

Ich habe mich für die zweite Variante entschieden, der Serveranbieter meiner Wahl ist [uberspace](http://uberspace.de/). Dort werden alle Daten gespeichert und über entsprechende Dienste verfügbar gemacht.   
Alle Erklärungen gehen davon aus dass die eigene Domain (meinedomain.de) auf dem uberspace abgelegt ist.

## verschlüsselte Verbindung

Grundlage um überhaupt sensible Daten wie Kalender und Kontakte selber zu hosten ist die Möglichkeit auf abhörsicherem Wege an diese heranzukommen - d.h. im Falle von calDAV und cardDAV per HTTPS.

Wenn man einfach mal versucht [https://meinedomain.de](https://meinedomain.de/) im Browser aufzurufen wird man vermutlich von einem Warnhinweis empfangen der auf eine unsichere Verbindung hinweist. Das ist nur teilweise richtig ... verschlüsselt ist die Verbindung schon, nur nicht mit einem Zertifikat das auf meinedomain.de ausgestellt ist, sondern auf den uberspace-Server auf dem meinedomain.de liegt.

Um schnell und einfach ein eigenes Zertifikat zu erhalten gibt es zum Glück seit kurzem die Initiatve [letsencrypt](https://letsencrypt.org/) und die uberspacer haben sich auch viel Mühe gegeben es ihren Kunden [möglichst einfach](https://wiki.uberspace.de/webserver:https#let_s-encrypt-zertifikate) zu machen dies zu nutzen.

Basierend auf der [Wikiseite der uberspacer](https://wiki.uberspace.de/webserver:https#let_s-encrypt-zertifikate):

    ~ $ uberspace-letsencrypt
    ~ $ letsencrypt certonly
    ~ $ uberspace-add-certificate -k ~/.config/letsencrypt/live/meinedomain.de/privkey.pem -c ~/.config/letsencrypt/live/meinedomain.de/cert.pem

Details bitte dem ausführlichen und aktuell gehaltenem Wikiartikel entnehmen und am besten auch gleich die dort beschriebene automatische Aktualisierung der Zertifikate einrichten.

## Kontakte und Kalender

Dieser Punkt ist noch mit relativ wenig Aufwand realisierbar.

### auf dem Server

Auf dem Server kommt [baikal](http://sabre.io/baikal/) zum Einsatz. Einfach per SSH auf den uberspace einloggen, ins html Verzeichnis wechseln, die [aktuelle Baikal-Zip](https://github.com/fruux/Baikal/releases) runterladen und entpacken

    ~ $ cd html
    ~ $ wget https://github.com/fruux/Baikal/releases/download/0.4.4/baikal-0.4.4.zip
    ~ $ unzip baikal-0.4.4.zip

Damit ist der schwierigste Teil schon geschafft, weiter geht es im Browser zu [https://meinedomain.de/baikal/html/admin](https://meinedomain.de/baikal/html/admin).

Sollte man hier auf die Meldung stoßen, dass eine aktuellere PHP-Version von nöten ist, so kann man dies einfach im uberspace in der Datei `etc/phpversion` einstellen ([genaueres hier](https://wiki.uberspace.de/development:php#php-version_einstellen)).

Auf der Baikal-Seite wird den Anweisungen gefolgt und am Ende sollte man mindestens ein Benutzerkonto mit Kalendern und einem Adressbuch haben.

### auf dem Androiden

Zur Kommunikation mit dem soeben eingerichteten calDAV / cardDAV Server kommt [DAVDroid](https://davdroid.bitfire.at/) zum Einsatz, es stellt eine Schnittstelle für jeden auf dem telefon installierten Kalender oder Kontaktverwalter zur Verfügung.   
Erhältlich ist es im [Google Store](https://play.google.com/store/apps/details?id=at.bitfire.davdroid&referrer=utm_source%3Dhomepage) oder auch im [F-Droid](https://f-droid.org/repository/browse/?fdfilter=davdroid&fdid=at.bitfire.davdroid).

Bei den Einstellungen wählt man "Login by URL" und wählt bei der URL `https://meinedomain.de/baikal/html/dav.php` aus. Login und Passwort hatte man bei der Einrichtung zuvor festgelegt.

Jetzt sollten die zuvor eingerichteten Kalender und das Adressbuch verfügbar sein.

### auf dem Desktop mit Thunderbird/Icedove

Mit Thunderbird ist die Verwendung der eingerichteten Kalender und Kontakte relativ einfach einzurichten.

Für Kalender richtet man einfach einen neuen CalDAV-Kalender ein:

*   `File --> New --> Calendar`
*   `on the Network`
*   `CalDAV` und als url `https://meinedomain.de/baikal/html/cal.php/calendars/LOGIN/KALENDERNAME`   
    LOGIN ist der bei der Einrichtung gewählte Benutzername, KALENDERNAME ist der Name des Kalenders (der erste heißt `default`)
*   wenn man mehrere Kalender eingerichtet hat, dann muss jeder Kalender separat eingebunden werden

Für die Kontakte via CardDAV ist ein zusätzliches AddOn erforderlich, ich habe mich für den [SOGo Connector](http://sogo.nu/download.html#/frontends) entschieden.   
Nachdem man das Addon installiert hat kann man das Adressbuch vom baikal-Server einbinden:

*   `Tools --> Adress Book`
*   im Adressbuch: `File --> New --> Remote Adress Book`
*   die url lautet `https://meinedomain.de/baikal/html/cal.php/addressbooks/LOGIN/ADRESSBOOKNAME`   
    LOGIN ist der bei der Einrichtung gewählte Benutzername, ADRESSBOOKNAME ist der Name des Adressbuchs (das erste heißt `default`)

### auf dem Desktop mit alternativen Clients

Wenn man nicht auf Thunderbird/Icedove zurückgreifen möchte kann man natürlich auch die eingerichteten Kalender und Adressbücher verwenden, nur ist es ein wenig umständlicher.

Der erste Schritt ist die Daten erstmal loakl abzuspeichern, dazu wird [vdirsyncer](https://github.com/pimutils/vdirsyncer) verwendet.

Um `vdirsync` unter Debian zum laufen zu bringen ist es wohl das einfachste <a>`pipsi`</a> zu verwenden (wenn man sich das script nur in einem virtualenv installieren will - wenn man sowieso z.B. khal mit pip3 installieren muss (weil man im Debian stable nicht mal einfach so auf python3 als default umstellen kann/sollte...), dann kann man sich den _Umweg_ über `pipsi` sparen, siehe unten).

    ~ $ curl https://raw.githubusercontent.com/mitsuhiko/pipsi/master/get-pipsi.py | python
    ~ $ pipsi install --python python3 vdirsyncer

Die `~/.config/vdirsyncer/config` kann dann folgendermaßen aussehen:

    [general]
    # A folder where vdirsyncer can store some metadata about each pair.
    status_path = ~/.vdirsyncer/status/  
    # CARDDAV
    [pair martin_contacts]
    a = martin_contacts_local  
    b = martin_contacts_remote  
    collections = ["from a", "from b"]

    # Synchronize the "display name" property into a local file (~/contacts/displayname).
    metadata = ["displayname"]

    [storage martin_contacts_local]
    type = filesystem  
    path = ~/contacts/  
    fileext = .vcf

    [storage martin_contacts_remote]
    type = carddav  
    url = https://meinedomain.de/baikal/html/card.php  
    username = ich  
    password = geheim

    # CALDAV
    [pair martin_calendar]
    a = martin_calendar_local  
    b = martin_calendar_remote  
    collections = ["from a", "from b"]

    # Calendars also have a color property
    metadata = ["displayname", "color"]

    [storage martin_calendar_local]
    type = filesystem  
    path = ~/calendars/  
    fileext = .ics

    [storage martin_calendar_remote]
    type = caldav  
    url = https://meinedomain.de/baikal/html/cal.php  
    username = ich  
    password = geheim  

Die Kalender landen nach einem

    ~ $ vdirsyncer sync

in `~/calendars/`, die Kontakte als vcf-Dateien in `~/contacts`.

Einen regelmäßigen Sync kann man per cronjob einrichten. Dabei bedeken dass man `cron` noch in dessen `PATH` mitteilen sollte, dass es in `~/.local/bin` (installiert mit `pipsi`) bzw. `/usr/local/bin/` (installiert mit `pip`) nach `vdirsyncer` gucken kann.

#### Kalender

Zur Darstellung und Verwaltung der Kalender kann man [khal](https://lostpackets.de/khal/index.html) verwenden.   
Da khal nur mit python3 läuft, muss zunächst python3-pip installiert werden.

    ~ $ sudo aptitude install python3-pip
    ...
    ~ $ pip3 install khal

Anschließend noch die notwendige Config-Datei `~/.config/khal/khal.conf` erstellen:

    [calendars]
        [[Beruflich]]
            path = ~/calendars/work/
            color = light red
        [[Alltag]]
            path = ~/calendars/daily/
            color = yellow
        [[Privat]]
            path = ~/calendars/default/
            color = light green
        [[Allgemein]]
            path = ~/calendars/generic/
            color = light blue

    [locale]
        timeformat = %H:%M
        dateformat = %d.%m.%Y
        longdateformat = %d.%m.%Y
        datetimeformat = %d.%m.%Y %H:%M
        longdatetimeformat = %d.%m.%Y %H:%M

    [default]
        highlight_event_days = True
        days = 10

#### Kontakte

Für Mails kann man [Claws Mail](http://www.claws-mail.org/) verwenden, leider ist dort der Support für Adressbücher nicht sehr zeitgemäß, es ist möglich ein vcf-File als Adressbuch einzubinden. Eine Bearbeitung ist mit Claws Mail leider nicht möglich.

Um aus den vielen vcf-Dateien in `~.contacts` eine einzelne zu erstellen können wir wieder `vdirsyncer` benutzen, dazu erweitern wir die `~/.config/vdirsyncer/config` um folgende Zeilen:

    [storage martin_contacts_local_addresses2016]
    type = filesystem  
    path = ~/contacts/addresses2016/  
    fileext = .vcf

    [storage claws_singlefile]
    type = singlefile  
    path = ~/contacts/contacts.vcf

    [pair]
    a = martin_contacts_local_addresses2016  
    b = claws_singlefile  
    collections = null  

Leider kann nur eine _collection_ (d.h. ein Adressbuch) in eine einzelne Datei (`singlefile`) geschrieben werden, in meinem Fall heißt das relevante Adressbuch _addresses2016_ und landet daher auch unter `~/contacts/addresses2016`.   
Wenn man mehrere Adressbücher verwaltet und dazu auch noch Claws Mail nutzt, dann muss man entsprechend mehrere Einzeldateien erzeugen lassen und einbinden.

## ToDos mittels Syncthing

Da ich bisher auf dem Androiden als auch auf dem Desktop das [todo.txt](http://todotxt.com/) Prinzip angewendet habe und mit [SimpleTask Cloudless](https://f-droid.org/repository/browse/?fdid=nl.mpcjanssen.simpletask) und der [todo.txt-CLI-Anwendung](https://github.com/ginatrapani/todo.txt-cli/releases)ganz gut zurecht kam wollte ich erstmal daran festhalten.

Es ist möglich in Kalendern (bzw. mittels CalDAV) sog. Tasks zu speichern. Eine einfache Möglichkeit ToDos zu verwalten (inkl. Fälligkeiten, Fortschritt usw.) - um diese Möglichkeit zu nutzen kann man die App [OpenTasks](https://github.com/dmfs/opentasks) verwenden (leider bisher ohne Kategorien, Filterung etc.).   
Da ich neben der einfachen Aufgabenverwaltung auch sämtliche anderen schnellen Notizen etc. mittels SimpleTask festhalte sind Kategorien und Filterung unerlässlich.

Für den Desktop soll wohl [todoman](https://gitlab.com/hobarrera/todoman) stark an todo.txt-CLI angelehnt sein, bis es auch einen Android Clienten der es mit SimpleTask aufnehmen kann gibt bleibe ich bei diesem.

Weil ich zuvor schon BTSync eingesetzt hatte wollte ich jetzt mal die quelloffene Alternative [Syncthing](https://syncthing.net/)ausprobieren.

### auf dem Server

coming soon, es sollte aber prinzipiell nach [dieser Anleitung](https://maxhaesslein.de/dachboden/syncthing-auf-uberspace/) klappen.

### Installation auf dem Desktop

Im Debian ist die Installation von Syncthing denkbar einfach, da ein [Repository inkl. Anleitung](https://apt.syncthing.net/) angeboten wird.   
Nach der Installation mit `apt-get` bzw. `aptitude` kann man zunächst einfach per Aufruf von `syncthing` den Service starten und es öffnet sich auch sogleich die Weboberfläche im Browser.

Wenn man den Syncthing-Service automatisch beim Login starten möchte, dann geht man einfach nach [dieser Anleitung](https://docs.syncthing.net/users/autostart.html#using-systemd) vor:

    ~ % mkdir .config/systemd/                                                                                   
    ~ % mkdir .config/systemd/user/                                                                              
    ~ % wget "https://raw.githubusercontent.com/syncthing/syncthing/master/etc/linux-systemd/user/syncthing.service" -O ~/.config/systemd/user/syncthing.service
    --2016-06-06 17:12:09--  https://raw.githubusercontent.com/syncthing/syncthing/master/etc/linux-systemd/user/syncthing.service
    Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.31.17.133  
    Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.31.17.133|:443... connected.  
    HTTP request sent, awaiting response... 200 OK  
    Length: 338 [text/plain]  
    Saving to: ‘/home/martin/.config/systemd/user/syncthing.service’

    /home/martin/.config/systemd/user/syncthi 100%[=======================================================================================>]     338  --.-KB/s   in 0s     

    2016-06-06 17:12:10 (4,85 MB/s) - ‘/home/martin/.config/systemd/user/syncthing.service’ saved [338/338]

    ~ %
    ~ % systemctl --user enable syncthing.service                                                                 
    Created symlink from /home/martin/.config/systemd/user/default.target.wants/syncthing.service to /home/martin/.config/systemd/user/syncthing.service.  
    ~ % systemctl --user start syncthing.service

### Installation auf dem Androiden

Auch hier gestaltet sich alles sehr einfach, entweder über den [Play Store](https://play.google.com/store/apps/details?id=com.nutomic.syncthingandroid) oder [F-Droid](https://f-droid.org/repository/browse/?fdid=com.nutomic.syncthingandroid) die App installieren und einrichten.

### Zusammenführung der Clients

Im Gegensatz zu BTSync müssen bei Syncthing zunächst alle Installationen (bzw. Devices) miteinander bekannt gemacht werden. Dazu müssen auf jedem Client die jeweils anderen eingetragen werden, die Identifikation erfolgt über die sog. Device-IDs (die kann man entweder umständlich abtippen oder man nutzt die Möglichkeit über QR-Codes und die Kamera des Androiden). Sobald man ein Device eingetragen hat dauert es nicht lange und das jeweils andere Gerät fragt nach Bestätigung - mit etwas Gedult kann man also recht schnell alle miteinander bekannt machen.   
Im nächsten Schritt können dann Verzeichnisse miteinander geteilt werden.

Sobald das eingerichtet ist sind wir auch schon fertig, evtl. sollte man noch das Syncronisationsinterval etwas an seine eigenen Bedürfnisse anpassen.

## ToDos mittels WebDAV

Wenn man nicht auf Syncthing für die Verteilung einer ToDos zurückgreifen möchte kann man alternativ auch einen WebDAV-Server auf dem uberspace einrichten. Die Vorgehensweise ist nachfolgend beschrieben.

Der Grund wieso ich im Endeffekt nicht auf diese Weise meine ToDos verwaltete ist bei der Software für Android zu finden: es wollte einfach nicht klappen mittels FolderSync per HTTPS auf den WebDAV-Server zuzugreifen. Auch ist Unison nicht die erwartete Wunderwaffe, wenn man gezwungenermaßen verschiedene Versionen einsetzt (lokal Debian stable vs. CentOS 6 auf dem uberspace).

### auf dem Server

Da wir schon bei Kalendern und Kontakten auf webDAV zurückgegriffen haben bietet es sich an auch für ToDos (bzw. geneuer gesagt eine todo.txt) diese Möglichkeit zu nutzen.   
Prinzipiell könnte man zwar auch _einfach_ einen Sync per SSH/rsync durchführen, aber beim Gedanken meinen uberspace SSH-Key (bzw. Passwort) auf dem Handy zu hinterlegen ist mir nicht ganz wohl.

Also webDAV auf dem uberspace einrichten:

Prinzipiell habe ich alles nach [dieser Anleitung](https://blog.cloudocs.de/webdav-auf-uberspace/)durchgeführt.   
Ein kleiner Fehler ist dem leider nicht erreichbaren Autor aber unterlaufen, und zwar sollte das `AuthUserFile`in der Apache-Config als auch beim erzeugen den gleichen Namen tragen.

Sollte der Beitrag irgendwann verschwinden, noch kurz die Schritte im Schnelldurchlauf:

`.htaccess` für eine neue Subdomain in `/var/www/virtual/NUTZER/SUB.DOMAIN.DE`

    RequestHeader edit Destination ^https: http:  
    RewriteEngine On  
    RewriteBase /  
    RewriteRule (.*) http://localhost:PORTNUMMER/$1 [P,QSA]  

Einen unbenutzten Port zwischen 61000 und 65535 auf dem Server suchen, mittels `/usr/sbin/ss -ln | fgrep PORTNUMMER`.

`KONFIGURATIONSVERZEICHNIS` (z.B. `~/etc/webdav/`) und `DATENVERZEICHNIS` (z.B. `~/data/webdav/`) anlegen.

Folgende `~/etc/webdav/apache2.conf` anlegen:

    Listen PORTNUMMER  
    ServerAdmin EMAIL  
    ServerRoot /var/www/virtual/NUTZER/SUB.DOMAIN.DE  
    ErrorLog ~/etc/webdav/error.log  
    CustomLog ~/etc/webdav/access.log "%t %u: %r -> %>s (%b)"  
    PidFile ~/etc/webdav/httpd.pid  
    DAVLockDB ~/etc/webdav/db.lock  
    LoadModule log_config_module /usr/lib64/httpd/modules/mod_log_config.so  
    LoadModule alias_module /usr/lib64/httpd/modules/mod_alias.so  
    LoadModule auth_basic_module /usr/lib64/httpd/modules/mod_auth_basic.so  
    LoadModule auth_digest_module /usr/lib64/httpd/modules/mod_auth_digest.so  
    LoadModule authn_file_module /usr/lib64/httpd/modules/mod_authn_file.so  
    LoadModule authn_alias_module /usr/lib64/httpd/modules/mod_authn_alias.so  
    LoadModule authn_default_module /usr/lib64/httpd/modules/mod_authn_default.so  
    LoadModule authz_user_module /usr/lib64/httpd/modules/mod_authz_user.so  
    LoadModule authz_owner_module /usr/lib64/httpd/modules/mod_authz_owner.so  
    LoadModule authz_groupfile_module /usr/lib64/httpd/modules/mod_authz_groupfile.so  
    LoadModule authz_default_module /usr/lib64/httpd/modules/mod_authz_default.so  
    LoadModule dav_module /usr/lib64/httpd/modules/mod_dav.so  
    LoadModule dav_fs_module /usr/lib64/httpd/modules/mod_dav_fs.so  
    NameVirtualHost *:PORTNUMMER  
    <VirtualHost *:PORTNUMMER>  
        ServerAlias SUB.DOMAIN.DE
        ServerAdmin EMAIL
        DocumentRoot ~/data/webdav
        <Directory ~/data/webdav/>
            Options +Indexes +SymLinksIfOwnerMatch MultiViews Includes
            AllowOverride All
        </Directory>
        <Location />
            DAV On
            AuthType Digest
            AuthName "SUB.DOMAIN.DE"
            AuthUserFile ~/etc/webdav/auth.dav
            Require valid-user
        </Location>
    </VirtualHost>  

Auf Fehler prüfen:

    ~ $ /usr/sbin/httpd -f ~/etc/wendav/apache2.conf -t

WebDAV Nutzer anlegen:

    ~ $ htdigest -c ~/etc/webdav/auth.dav SUB.DOMAIN.DE WEBDAVNUTZER

Service anlegen, damit die Apache Instanz auch nach einem reboot läuft:

    ~ $ uberspace-setup-service webdav /usr/sbin/httpd -f ~/etc/webdav/apache2.conf -D FOREGROUND

Ferig ist der webDAV, der unter der neuen subdomain erreichbar ist. Wichtig ist zu beachten, dass wir immer per https zugreifen sollten, da ansonsten die Logindaten unverschlüsselt übertragen werden.

### auf dem Desktop

Für die Syncronisation der todo.txt zwischen Desktop und webDAV (bzw. genauer gesagt dem Verzeichnis `~/data/webdav/todo`) verwende ich einfach <a>unison</a> über SSH. Der SSH Key liegt sowieso auf dem Rechner und auf dem uberspace ist unison installiert - diesmal galt doch _wieso kompliziert wenn es auch einfach geht_.   
Über die GUI (`unison-gtk`) kann man sich ein Profil anlegen, welches dann einfach in der Konsole mittels `unison PROFILNAME` aufgerufen werden kann. Ein regelmäßiger Abgleich kann mittels cron erreicht werden.

In Zukunft muss ich mich mal mit dem `-repeat watch`Parameter von unison beschäftigen, es scheint möglich zu sein einen sync anzuwerfen wenn Änderungen durchgeführt werden...

### auf dem Androiden

Auf dem Androiden sollte der Sync mit einem WebDAV-Verzeichnis eigentlich mit FolderSync erfolgen, aber leider weigert der sich per https auf den WebDAV-Server zuzugreifen und ist nicht sehr auskunftsfreudig was genau ihm denn nicht passt... vom Desktop komme ich per `davfs` (über `/etc/fstab`) als auch mit `cadaver`problemlos auf den webDAV-Speicher drauf, an der Server Einrichtung sollte es also (eigentlich) nicht liegen.
