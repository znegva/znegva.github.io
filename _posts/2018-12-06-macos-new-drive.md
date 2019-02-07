---
layout: post
title: Replace hard-drive without reinstalling MacOS
tags: macOS Hardware
---

__Goal__: Replace the old 500GB HDD of the Mac Mini with a new 500GB SSD without
the need of reinstalling MacOS again (see why [here]({% post_url 2016-12-05-macos-fuer-newbies %})).

Since I wanted to avoid a new installation at all costs, the old hard disk had
to be completely cloned to the new one.
Fortunately, the hard disks are the same size, so there is no need to adjust the
partitions or anything like that.

Fortunately all needed programs are already shipped with MacOS, our tool of choice is
the _Disk Utility.app_ which can be found in _Applications --> Utilities_.
To be able to clone our current hard drive the first thing we need is a way to
connect both hard-drives at the same time, I decided to use a simple USB hard-disc case.

Since we want an exact clone we need to be able to unmount the drive, fortunately
the _Disk Utility.app_ is also available in Recovery Mode.

Steps:

- start your Mac in Recovery mode by pressing [Command (⌘)-R at the startup](https://support.apple.com/en-us/HT201255)
- open the _Disk Utility_ from the menu
- __important__: even if you will be erasing everything on the new drive, you still need to format it!  
  otherwise the _Disk Utility_ wont let you _restore_ your current drive to the new one  
  it will instead grumble about your old drive being formatted in some wrong way  
  formatting can also be done with the _Disk Utility_ (who would have thought that?)
- now select your new drive (connected via USB) and press __Restore__
- select the old drive as source for restoring
- get yourself something else to to, this will keep some time

Now its time to replace the hard disc, you can <s>google</s> [find many HowTos online](https://www.startpage.com/do/dsearch?query=mac+mini+replace+hard+drive).  
- be careful with the metal cover and your RAM!
- the HowTos tell you you need to lift some tape and unplug the WiFi - I could just put aside the metal cover without unplugging the WiFi.
- one very helpful hint was to use a plastic card as kind of ramp to insert the drive - you'll know what I mean when you're at this point.

After reassembly everything started up as usual, but waaay faster - great!

I kept the old drive untouched for some time, you never know :)
