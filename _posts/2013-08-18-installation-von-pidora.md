---
title: Installation von Pidora
date: 2013-08-18T17:18:10+02:00
layout: post
---

Vergangenen Freitag ist am Ende des Arbeitstages mein Pi in meinen Rucksack
und damit mit nach Hause gewandert, um am Wochenende ein paar neue Dinge
ausprobieren zu können. Wenn ich gerade so aus dem Fenster schaue eine gute
Entscheidung - es regnet leider Gottes.
Testweise möchte ich neben Raspbian eine andere Linux-Distribution ausprobieren. Als Kandidat habe ich Pidora auserkoren. Die Distribution kann über die <a href="http://pidora.ca/" title="Raspberry Pi Fedora Remix" target="_blank">Pidora-Webseite</a> bezogen werden. Nachfolgend beschreibe ich die Installation basierend auf Version 18-r2c.


Nach
dem Herunterladen des circa 490 MByte großen `zip`-Archivs `pidora-18-r2c.zip`
wird dessen Integrität anhand der `sha1`-Prüfsumme festgestellt.
Die `sha1`-Prüfsumme für die heruntergeladene Datei ermittelt man per

```shell
$ sha1sum pidora-18-r2c.zip
c43178ea18063928de21daf441aa10152d4ce4f9  pidora-18-r2c.zip
```

Ist die Prüfung erfolgreich verlaufen, kann das `zip`-Archiv entpackt werden
und der Inhalt anhand der `MD5`-Prüfsumme gegengeprüft werden.

```shell
$ unzip pidora-18-r2c.zip
Archive:  pidora-18-r2c.zip
  inflating: pidora-18-r2c.img
 extracting: pidora-18-r2c.img.md5sum

$ md5sum -c pidora-18-r2c.img.md5sum
pidora-18-r2c.img: OK
```

Das aus dem Archiv extrahierte Image muss nun auf die SD-Speicherkarte kopiert
werden. Falls die Karte bereits eingebunden ist, ist sie zunächst auszuhängen.
Ist dies geschehen, kann das heruntergeladene Image per `dd` auf die
Speicherkarte überspielt werden.

```shell
dd bs=4M if=pidora-18-r2c.img  of=/dev/sdb
```

Nachträglich noch ein `sync` und schon ist die Speicherkarte bereit für den
Einsatz im Pi! Pidora startet in eine graphische Oberfläche (Firstboot), über
die man weitere Einstellungen vornehmen kann. Will man das Ganze lieber per
`ssh` regeln, so geht dies als Nutzer `root` mit dem Passwort `raspberrypi`.
Um die Mehrfachausführung von `firstboot` zu vermeiden, wird vor dessen Aufruf
noch ein wenig aufgeräumt.

```shell
$ ssh root@myraspberrypi -X
root@myraspberrypi's password:
Last login: Thu Jun 20 14:51:38 2013 from XXX.XXX.XXX
[root@raspi ~]#  ps -ef | grep -i firstboot
root       130     1  2 14:46 ?        00:00:14 python /usr/sbin/firstboot
root       709   693  0 14:55 pts/0    00:00:00 grep --color=auto -i firstboot
[root@raspi ~]# kill 130
[root@raspi ~]# firstboot
```
