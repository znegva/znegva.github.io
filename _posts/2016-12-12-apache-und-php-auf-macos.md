---
layout: post
title: Apache und PHP in macOS einrichten
lang: de_DE
tags: macOS PHP Apache
---

macOS bringt bereits recht aktuelle Versionen vom Apache Webserver und PHP mit, eine Installation aus externen Quellen ist also nicht notwendig.

``` sh
~ % httpd -v
Server version: Apache/2.4.23 (Unix)  
Server built:   Aug  8 2016 16:31:34  
~ % php -v
PHP 5.6.25 (cli) (built: Sep 19 2016 15:45:41)  
Copyright (c) 1997-2016 The PHP Group  
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies  
~ %
```

## grundlegende Befehle

*   `sudo apachectl start` zum starten des Webservers
*   `sudo apachectl stop` zum beenden des Webservers
*   `sudo apachectl restart` zum neustarten des Webservers

## Nutzerverzeichnisse einrichten

Es bietet sich an individuelle Nutzerverzeichnisse für die eigenen HTML bzw PHP Projekte zu verwenden, auf diese Weise muss nicht dauerhaft mit root-Rechten im `/Library/WebServer/Documents/` geschrieben werden.

Dazu wird zunächst ein Verzeichnis `Sites` für den aktuellen Nutzer erstellt und anschließend eine entsprechende conf-Datei angelegt

``` sh
~ % mkdir Sites
~ % whoami
martin  
~ % sudo vim /etc/apache2/users/martin.conf
```

Hier dann folgendes eintragen

``` conf
<Directory "/Users/martin/Sites/">  
AllowOverride All  
Options Indexes MultiViews FollowSymLinks  
Require all granted  
</Directory>  
```

Anschließend noch die gewünschten Module bzw. Includes aktivieren (`#` davor entfernen):

```
~ % sudo vim /etc/apache2/httpd.conf
```

Ich habe folgende Module aktiviert (insbesondere PHP) und das userdir-conf entkommentiert:

``` apache
LoadModule authz_core_module libexec/apache2/mod_authz_core.so  
LoadModule authz_host_module libexec/apache2/mod_authz_host.so  
LoadModule userdir_module libexec/apache2/mod_userdir.so  
LoadModule include_module libexec/apache2/mod_include.so  
LoadModule rewrite_module libexec/apache2/mod_rewrite.so  
LoadModule php5_module libexec/apache2/libphp5.so

Include /private/etc/apache2/extra/httpd-userdir.conf  
```

in eben dieser `/private/etc/apache2/extra/httpd-userdir.conf` dann noch folgendes unkommentieren:

``` apache
Include /private/etc/apache2/users/*.conf  
```

und den Apachen neu starten

``` sh
~ % apachectl restart
```

Ab jetzt sollten Dateien aus `~/Sites` unter `http://localhost/~martin/` erreichbar sein.

Sollte der Webserver bzw PHP schreibend auf angelegte Dateien zugreifen müssen, dann müssen entsprechende verzeichnisse noch der Gruppe `_www`zugewiesen werden, und der Gruppe Schreibrechte gegeben werden.

``` sh
~/Sites % sudo chgrp -R _www ./TEST
~/Sites % chmod -R g+w TEST
```
