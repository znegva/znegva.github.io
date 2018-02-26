---
layout: post
title: Android Emulator - No Internet Connection
tags: Android
---


Now that we are all gently forced to use Android Studio to manage the Android
SDKs and Emulators we can happily click our stuff together, until something isn't working.

My newly created Android Emulator had connected to the default WiFi (_AndroidWifi_),
but had no Internet connection.  
And the _Android Virtual Device Manager_ clickGUI couldn't help me, even the _Advanced Settings_ only offered which Camera the emulated device shall use o.O

After some searching the most likely problem seems to be, that the wrong network
device had been allocated to the emulator and therefore a wrong DNS server is
used - but I was not able to find out how to change this from within the _nice_ GUI.

Time for the console:
```
~ % emulator -list-avds
Nexus_5X_API_25
Nexus_7_2012_API_25
~ % emulator -avd Nexus_5X_API_25 -dns-server 8.8.8.8
...
...
~ % where emulator
/Users/martin/Library/Android/sdk/tools/emulator
```
