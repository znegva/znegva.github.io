---
layout: post
title: Sound Output Device unter Debian festlegen
lang: de_DE
tags: Debian
---

Wenn man z.B. einen HDMI-Fernseher an seinen Laptop angeschlossen hat, dann möchte man evtl. auch dessen Boxen zur Soundausgabe benutzen.   
Wenn man jetzt auch noch kein Fan von Pulseaudio oder überbordenden Desktopumgebungen mit Klickinterfaces für alles, dann muss die Shell herhalten um das Sound Output Device festzulegen.

Eine Liste der verfügbaren Devices liefert `aplay -l` - mit den dort gefundenen Informationen kann man sich eine `~/.asoundrc` erstellen.

``` sh
~ % aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: PCH [HDA Intel PCH], device 0: CX20590 Analog [CX20590 Analog]  
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 3: HDMI 0 [HDMI 0]  
  Subdevices: 1/1
  Subdevice #0: subdevice #0
~ % more .asoundrc
pcm.!default{  
        type hw
        card 0
        device 3
}
~ %
```

Ich habe gerade den HDMI Ausgang als Output festgelegt.

Um das Ganze etwas komfortabler zu haben habe ich mir ein kurzes Script gebastelt, dass automatisch die `~/.asoundrc` für mich erzeugt.

``` bash
# !/bin/bash
# @author: martin

usage="$(basename "$0") [-l] [-s] [-d] [device] -- program to set sound output device  
where:  
    -l      list available devices (output of aplay -l)
    -s      show current ~/.asoundrc
    -d      delete ~/.asoundrc
    device  set ~/.asoundrc for any predefined device
    "

type="hw"  
card="0"  
device="0"

case "$1" in  
    "-l")
        aplay -l
        exit
        ;;
    "-s")
        echo ~/.asoundrc :
        more ~/.asoundrc
        exit
        ;;
    "-d")
        rm ~/.asoundrc
        echo ~/.asoundrc deleted
        exit
        ;;
    "HDMI"|"hdmi")
        type="hw"
        card="0"
        device="3"
        ;;
    "buildin")
        type="hw"
        card="0"
        device="0"
        ;;
    *)
        echo "$usage"
        exit
        ;;
esac

cat > ~/.asoundrc << EOF  
pcm.!default{  
        type $type
        card $card
        device $device
}
EOF

echo new ~/.asoundrc written
```
