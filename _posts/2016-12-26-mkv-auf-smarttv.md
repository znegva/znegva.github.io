---
layout: post
title: mkv auf Samsung smart TV abspielen
lang: de_DE
tags: Hardware aaargh
---

Manche Samsung smart TVs sind offenbar nicht ganz so smart wenn es um Videoinhalte im [MKV Container](https://de.wikipedia.org/wiki/Matroska) geht - sie weigern sich einfach es abzuspielen.

Abhilfe schafft es, die Video- und Audioinhalte einfach in einen anderen Container ([MP4](https://de.wikipedia.org/wiki/MP4)) zu packen:

    ~ % ffmpeg -i FILM.mkv -c copy -f mp4 FILM.mp4

Das geht sehr fix, da alle Streams einfach nur ins neue Containerformat kopiert werden - danach akzeptiert der Samsung TV es.
