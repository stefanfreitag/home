---
title: Backup von einem Android Tablet
date: 2012-12-29T20:55:23+01:00
author: Stefan Freitag
---
Von Zeit zu Zeit erstelle ich von den Daten auf meinem Rechner ein Backup. Vor
kurzem sind Backups von meinem Smartphone und dem Tablet dazugekommen. Um von
den letztgenannten ein Backup erstellen zu können, benötigt man einen PC
sowie Android in Version 4.0 oder neuer auf den Endgeräten.

Auf den PC ist das Android SDK zu installieren. Dieses bringt das Werkzeug
`adb` (Android Debug Bridge) mit sich, über welches das Backup sowie ein
späteres Wiederherstellen möglich ist.

Ist das SDK auf dem PC eingerichtet, können die notwendigen Einstellungen auf
dem Android-basierten Endgerät vorgenommen werden. Diese sind überschaubar und
bestehen lediglich aus dem Aktivieren des USB Debugging. Im Anschluss daran wird
das Endgerät mittels eines USB-Kabels an den PC angebunden.

Ob das Endgerät, in meinem Fall das Tablet, durch `adb` aufgefunden wurde,
zeigt der Aufruf

```plain
adb devices
```

Sofern der Aufruf keine Geräte auflistet, liegt leider ein Problem vor.
Das Backup an sich geht, nachdem das Gerät erkannt wurde, einfach von statten:

```plain
adb backup -apk -shared -all -f backup.ab
```

Dabei werden alle installierten Apps sowie der Inhalt der SD-Speicherkarte
gesichert. Bevor jedoch das Backup beginnt, muss man diesem am Endgerät
zustimmen.

Die Wiederherstellung der Daten geschieht analog per

```plain
adb restore backup.ab
```
