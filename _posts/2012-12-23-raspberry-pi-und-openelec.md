---
title: Raspberry Pi und OpenELEC
date: 2012-12-23T20:45:25+01:00
author: Stefan Freitag
tags: 
  - raspberry-pi
  - openelec
---

Mein erstes Projekt mit dem Raspberry Pi besteht darin, diesen als
Multimedia-Box einzusetzen. Hierzu verwende ich OpenELEC. Auf der
[OpenElec-Homepage](http://openelec.tv/) wird die Software wie folgt
beschrieben:

> Open Embedded Linux Entertainment Center, or OpenELEC for short, is a small
> Linux distribution built from scratch as a platform to turn your computer
> into a complete XBMC media center. OpenELEC is designed to make your system
> boot as fast as possible and the install is so easy that anyone can turn a
> blank PC into a media machine in less than 15 minutes.

Die Software kann kostenlos über diesen [Link](http://openelec.tv/get-openelec)
heruntergeladen werden. Nach dem Download ist die Datei einfach zu entpacken per

```shell
tar xzf OpenELEC-RPi.arm-2.95.6.tar.bz2
```

Nun wird in das neue Verzeichnis gewechselt. In diesem liegt ein Skript, über
welches sich die SDCard präparieren lässt. Bei der Ausführung des Skripts ist
auf die passende Angabe für das Device der SDCard (z.B. _/dev/sdb_) zu
achten.

```shell
cd OpenELEC-RPi.arm-2.95.6
sudo ./create_sdcard /dev/sdb
```

Ist der Vorgang abgeschlossen, kann die SDCard mit dem Raspberry Pi verwendet
werden. Hier noch Nutzername und Passwort:

```shell
Nutzer: root
Passwort: openelec
```
