---
layout: post
title: Preparing images for the web
tags: snippets
---

Preparing images from your camera or smartphone for using them in a weblog
or somewhere else on the web can be a mess.

Since I am not much into reworking the images I like to use automated procedures,
and therefore tend to use the command-line as much as possible.  
All the tools mentioned in this post here can be installed using the typical
package managers of your system (e.g. [brew](https://brew.sh/) for macOS or
apt/aptitude for Debian).

Here are my typical steps for recurring tasks:

## resizing images

Tool of choice is `convert` from the [ImageMagick](http://www.imagemagick.org/script/index.php) tools.

``` sh
 % find . -name '*.jpg' -exec convert {} -resize 1200x1200 -quality 80 -auto-orient {} \;
```

Make sure to use `auto-orient` especially if you took your photos with an iPhone!

__Attention:__ this overwrites your original images, so make sure you are working on copies!

### macOS

MacOS ships his own tool for such tasks: `sips`. It is working in a similar way, but the
generated images are way larger and look worse...

``` sh
 % find -name '*.jpg' -exec sips -Z 1000 {} --out {} \;
```

### graphical

The macOS Preview application offers a build-in batch resizer:
 * mark all images
 * open in Preview (Apple+O)
 * select all thumbnails (with same orientation) in sidebar
 * select _Tools_ --> _Adjust Size_
 * now you can select either width or height (thats why you only selected thumbnails with same orientation)
 * the file sizes with this method are also worse than with ImageMagick

## removing EXIF data

To remove EXIF-data (interesting stuff such as shutter speed, aperture but also
    scary stuff like GPS, exact date and time of creation, Camera serial number)
    we use the tool `exiftool` (what a appropriate name).

To remove all EXIF data of all `jpg` files in your folder just do:

``` sh
 % exiftool -all= -overwrite_original *.jpg
```

If you are using an iPhone, make sure to `-auto-orient` the image (see above) before
removing the EXIF-data.


## renaming many files in one go

`exiftool` can also be used to rename files based on EXIF-data.  
This can be helpful for renaming from the format of e.g. Canon-cameras (`IMG_1234.jpg`)
to a more readable format containing date and time (`IMG_20171123_164901.jpg`):

``` sh
% exiftool -P '-filename<CreateDate' -d IMG_%Y%m%d_%H%M%S%%-c.%%le *
```

For more generic renaming most OS/file-managers offer more or less good
solutions, I really like [rangers](http://nongnu.org/ranger/) `:bulkrename` command.


## deleting redundant folders and files

When you are switching from any previous CMS or page-builder it can be handy to
be able to delete all subdirectories and files of a given directory (e.g.
    autogenerated thumbnails).

``` sh
% find . -mindepth 2 -type d -exec rm -r {} \;
```
