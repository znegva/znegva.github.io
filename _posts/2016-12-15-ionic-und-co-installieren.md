---
layout: post
title: ionic, XCode und Android SDK sowie Emulatoren unter macOS einrichten
lang: de_DE
tags: macOS ionic Android iOS
---

[Ionic](https://ionicframework.com/) ist ein Open-Source-Framework zur Erstellung von Hybrid-Apps (also iOS, Android und weitere) auf der Basis von HTML5, CSS, Sass und JavaScript und basiert auf AngularJS und Apache Cordova. Ionic baut auf [node.js](https://nodejs.org/) auf, dessen Paketmanager ist wiederum [npm](https://www.npmjs.com/).

## Installation

Nachdem im vorigen Post erläutert wurde wie man node und npm einrichtet geht es hier weiter mit der Installation von ionic selbst:

``` sh
~ % npm install ionic -g --loglevel info
~ % npm install cordova -g --loglevel info
```

## Vorbereitungen iOS

Um mit ionic Apps für iOS entwicklen zu können muss zunächst einmal XCode über den App Store installiert werden - da dies mit einem ca. 4,5 GB Download verbunden ist sollte man das je nach Internetanbindung ein wenig planen.   
Mit XCode kommt die Laufzeitumgebung und auch die Emulatoren fürs iPhone etc. die für die Entwicklung benötigt werden.

Damit sollte auch schon alles für iOS eingerichtet sein.

## Projekt anlegen

---
__Update 2017__: aktuell ist ionic Version 3.9, die Anleitung hier ist also vermutlich
veraltet!  

---


``` sh
~ % mkdir temp
~ % cd temp
~/temp % ionic start --v2 myApp tabs
******************************************************
 Dependency warning - for the CLI to run correctly,
 it is highly recommended to install/upgrade the following:

 Install ios-sim to deploy iOS applications.`npm install -g ios-sim` (may require sudo)
 Install ios-deploy to deploy iOS applications to devices.  `npm install -g ios-deploy` (may require sudo)

******************************************************
Creating Ionic app in folder /Users/martin/temp/myApp based on tabs project  
Downloading: https://github.com/driftyco/ionic2-app-base/archive/master.zip  
[=============================]  100%  0.0s
Downloading: https://github.com/driftyco/ionic2-starter-tabs/archive/master.zip  
[=============================]  100%  0.0s
Installing npm packages...  
npm WARN prefer global node-gyp@3.4.0 should be installed with -g

> fsevents@1.0.15 install /Users/martin/temp/myApp/node_modules/fsevents
> node-pre-gyp install --fallback-to-build

[fsevents] Success: "/Users/martin/temp/myApp/node_modules/fsevents/lib/binding/Release/node-v48-darwin-x64/fse.node" is installed via remote

> node-sass@3.10.1 install /Users/martin/temp/myApp/node_modules/node-sass
> node scripts/install.js

Start downloading binary at https://github.com/sass/node-sass/releases/download/v3.10.1/darwin-x64-48_binding.node  
Binary downloaded and installed at /Users/martin/temp/myApp/node_modules/node-sass/vendor/darwin-x64-48/binding.node

> node-sass@3.10.1 postinstall /Users/martin/temp/myApp/node_modules/node-sass
> node scripts/build.js

"/Users/martin/temp/myApp/node_modules/node-sass/vendor/darwin-x64-48/binding.node" exists.
 testing binary.
Binary is fine; exiting.

[...]

Adding initial native plugins  
Initializing cordova project without CLI  
-
Adding in iOS application by default  
Saving your Ionic app state of platforms and plugins  
Saved platform  
Saved plugins  
Saved package.json

♬ ♫ ♬ ♫  Your Ionic app is ready to go! ♬ ♫ ♬ ♫

Some helpful tips:

Run your app in the browser (great for initial development):  
  ionic serve

Run on a device or simulator:  
  ionic run ios[android,browser]

Test and share your app on device with Ionic View:  
  http://view.ionic.io

Build better Enterprise apps with expert Ionic support:  
  http://ionic.io/enterprise

New! Add push notifications, live app updates, and more with Ionic Cloud!  
  https://apps.ionic.io/signup

New to Ionic? Get started here: http://ionicframework.com/docs/v2/getting-started

~/temp %
```

Das ging schonmal gut, die iOS Platform hat er schon von alleine hinzugefügt - einzig die beiden iOS-spezifischen Pakete liefern wir noch nach, aber ohne sudo ;)

``` sh
~ % npm install -g ios-sim
~ % npm install -g ios-deploy
```

### für iOS kompilieren und im Emulator starten

``` sh
~/temp % cd myApp
~/temp/myApp % ionic emulate ios

> ionic-hello-world@ ionic:build /Users/martin/temp/myApp
> ionic-app-scripts build

[03:51:07]  ionic-app-scripts 0.0.47
[03:51:07]  build dev started ...
[03:51:07]  clean started ...
[03:51:07]  clean finished in 8 ms
[03:51:08]  copy started ...
[03:51:08]  transpile started ...
[03:51:11]  transpile finished in 3.33 s
[03:51:11]  webpack started ...
[03:51:11]  copy finished in 3.56 s
[03:51:22]  webpack finished in 11.15 s
[03:51:22]  sass started ...
[03:51:24]  sass finished in 1.66 s
[03:51:24]  build dev finished in 16.19 s
Building project  : /Users/martin/temp/myApp/platforms/ios/myApp.xcodeproj

    Configuration : Debug
    Platform      : emulator

Build settings from command line:

    ARCHS = i386
    CONFIGURATION_BUILD_DIR = /Users/martin/temp/myApp/platforms/ios/build/emulator
    SDKROOT = iphonesimulator10.1
    SHARED_PRECOMPS_DIR = /Users/martin/temp/myApp/platforms/ios/build/sharedpch
    VALID_ARCHS = i386

[ ratter ratter ]

** BUILD SUCCEEDED **

No target specified for emulator. Deploying to iPhone-SE, 10.1 simulator  
```

Juchu, nach laaanger Ladezeit ist das künstliche iPhone fertig und alles funktioniert.

![iPhone Emulator mit dem ionic Tabs Beispielprojekt](/assets/myApp.iOS.jpg)

## Vorbereitungen Android

Neben iOS wollen wir natürlich auch für Android kompilieren und emulieren können, dazu benötigen wir Java (und eine schnelle Internetverbindung) und das Android SDK (Updates unten beachten!):

``` sh
~ % brew cask install java
~ % brew install android-sdk
~ % android
```

der letzte Befehl startet den Android SDK Manager, eine grafische Oberfläche unter der wir die benötigten Versionen der SDK Tools, SDK Platform, Build Tools und System-Images (Emulatoren) herunterladen können.   
Auch hier wieder mit Bedacht auswählen, insbesondere wenn die Internetanbindung nicht die schnellste ist!

<div class="attention" markdown="1">

__Update Q3 2017__: aktuell kann die gesamte Verwaltung der SDKs und Emulatoren nur noch
über Android Studio halbwegs erträglich geregelt werden :-/.  

__Update 2018__: `brew cask install java` installiert immer die aktuellste Java / Java-SDK Version - d.h. aktuell Java 10.  
Cordova bzw. das Android SDK benötigt aber zwingend Java8:  

``` sh
~ % brew tap caskroom/versions
~ % brew cask install java8
```
</div>



Wichtig ist auch unter Extras der _Intel X86 Emulator Accelerator (HAXM installer)_ installiert wird, ohne den funktioniert der Emulator nicht.

HAXM muss dann auch noch händisch installiert werden:

``` sh
~ % open /usr/local/Cellar/android-sdk/24.4.1_1/extras/intel/Hardware_Accelerated_Execution_Manager/IntelHAXM_6.0.5.dmg
```

dort den Anweisungen folgen.

Für die Android Emulation müssen wir uns zunächst ein Android Virtual Device (AVD) erstellen und starten.   
Das geht über den _Android Virtual Device (AVD) Manager_ (wer hätte das gedacht) - den starten wir mittels:

``` sh
~ % /usr/local/Cellar/android-sdk/24.4.1_1/bin/android avd
```

Hier ein Device erstellen und starten - als _CPU/ABI_ sollten wir eine Intel X86 CPU wählen, weil dann die Emulation besser läuft (genau dazu brauchten wir den HAXM). Und tatsächlich startet der Android Emulator deutlich fixer als das virtuelle iPhone.

Jetzt kann es aber endlich losgehen:

## für Android kompilieren und im Emulator starten

Android als Zielplattform hinzufügen:

``` sh
~/temp/myApp % ionic platform add android

Adding android project...

Creating Cordova project for the Android platform:

    Path: platforms/android
    Package: com.ionicframework.myapp443664

    Name: myApp
    Activity: MainActivity

    Android target: android-24

Subproject Path: CordovaLib

Android project created with cordova-android@6.0.0

Installing "cordova-plugin-console" for android

ANDROID_HOME=/usr/local/Cellar/android-sdk/24.4.1_1

JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_112.jdk/Contents/Home

Subproject Path: CordovaLib

Starting a new Gradle Daemon for this build (subsequent builds will be faster).

Incremental java compilation is an incubating feature.

:clean

:CordovaLib:clean

[ ratter ratter ]

BUILD SUCCESSFUL

Total time: 1.536 secs
```

kompilieren und an den schon laufenden Emulator senden:

``` sh
~/temp/myApp % ionic emulate android

> ionic-hello-world@ ionic:build /Users/martin/temp/myApp
> ionic-app-scripts build

[04:39:41]  ionic-app-scripts 0.0.47
[04:39:41]  build dev started ...
[04:39:41]  clean started ...
[04:39:41]  clean finished in 97 ms
[04:39:41]  copy started ...
[04:39:41]  transpile started ...
[04:39:50]  transpile finished in 8.97 s
[04:39:50]  webpack started ...
[04:39:51]  copy finished in 10.09 s
[04:40:03]  webpack finished in 13.58 s
[04:40:03]  sass started ...
[04:40:06]  sass finished in 2.50 s
[04:40:06]  build dev finished in 25.25 s
ANDROID_HOME=/usr/local/Cellar/android-sdk/24.4.1_1

JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_112.jdk/Contents/Home

Subproject Path: CordovaLib

Incremental java compilation is an incubating feature.

:preBuild
 UP-TO-DATE

[ ratter ratter ]

Built the following apk(s):  
    /Users/martin/temp/myApp/platforms/android/build/outputs/apk/android-debug.apk

Using apk: /Users/martin/temp/myApp/platforms/android/build/outputs/apk/android-debug.apk

Package name: com.ionicframework.myapp443664

INSTALL SUCCESS

LAUNCH SUCCESS

~/temp/myApp %
```

Neben dem virtuellen iPhone ist auch das virtuelle Android bereit für die Arbeit

![Android und iOS Emulatoren Seite an Seite](/assets/myApp.both.png)

## Ergänzung

Es kann vorkommen, dass der PATH zu den Android-Tools vergessen wird (ich glaube das passierte als das android-sdk-Paket innerhalb von brew von core nach cask verschoben wurde).

In diesem Fall einfach von Hand innerhalb der `~/.zshrc` bzw. `~/.bashrc` ergänzen:

``` conf
ANDROID_HOME=/usr/local/opt/android-sdk  
PATH=$PATH:/usr/local/opt/android-sdk/tools:/usr/local/opt/android-sdk/platform-tools  
```

### Testen auf dem Android Gerät

Wenn die App irgendwann auch auf dem echten (Android)-Device getestet werden soll kann es passieren dass ionic die `apk` immer nur auf den Emulator kopieren möchte, obwohl ein echtes Gerät angeschlossen ist.   
In diesem Fall einfach das Zielgerät dazusagen:

``` sh
~/temp/myApp % adb devices
List of devices attached  
emulator-5554    device  
TA32403NHY       device

~/temp/myApp % ionic run android --target=TA32403NHY
```

### Debugging auf dem Android Gerät

Zum Debuggen kann man optimal die Developer Tools des Chrome-Browsers benutzen, einfach Developer-Tools öffnen, im dortigen Menü unter **More Tools --> Remote Devices** findet man eine Liste der erkannten angeschlossenen Gerät. Der Rest erklärt sich von allein.
