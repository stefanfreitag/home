---
title: Moto G Auto PowerOn
date: 2016-07-23T22:28:49+02:00
layout: post
---

Mein aktuelles Smartphone ist ein [Moto
G](https://de.wikipedia.org/wiki/Motorola_Moto_G) der ersten Generation (XT
1032), allerdings nicht mehr mit dem Original-Betriebssystem. Dieses ist vor
knapp anderthalb Monaten
[CyanogenMod](http://web.archive.org/web/20160831091147/http://wiki.cyanogenmod.org:80/w/Main_Page)
13 gewichen. Und ich bin echt zufrieden, vor allem was die Performance angeht.

Für ein Projekt habe ich mein Telefon als Testkandidaten bestimmt und wollte ein
bestimmtes Feature testen. Wie man das so kennt, müssen Smartphones über kurz
oder lang zum Aufladen an die Steckdose. Schließe ich meines im ausgeschalteten
Zustand an, so erhalte ich eine Information über den Ladezustand der Batterie.

Im Internet gibt es unter anderem auf [StackOverflow](http://stackoverflow.com/)
Hinweise und teils auch Anleitungen, wie man über das Anschließen an eine
Stromquelle ein Smartphone einschaltet. Genau dieses Feature will ich testen.
Hierzu muss ich zunächst in den Bootloader des Telefons wechseln:

```shell
adb devices
List of devices attached
TA93004WAT      device

adb reboot bootloader
```

Der erste Befehl prüft, ob das Telefon von
[adb](https://developer.android.com/studio/command-line/adb.html) gesehen wird,
der zweite startet das Telefon im erforderlichen Modus.

Mit dem ersten `fastboot`-Befehl prüfe ich, ob ein Zugriff auf das Telefon
möglich ist. Der zweite liefert eine Übersicht über die vorhandenen
Kommandos. Das relevante Kommando `off-mode-charge` ist bei meinem Moto G
verfügbar.

```shell
sudo fastboot devices
TA93004WAT      fastboot

$ sudo fastboot oem help
...
(bootloader) config...
(bootloader) fb_mode_set
(bootloader) fb_mode_clear
(bootloader) bp-tools-on
(bootloader) bp-tools-off
(bootloader) qcom-on
(bootloader) qcom-off
(bootloader) unlock
(bootloader) lock
(bootloader) get_unlock_data
(bootloader) cid_prov_req
(bootloader) read_sv
(bootloader) off-mode-charge
OKAY [  0.022s]
finished. total time: 0.022s
```

Nachdem dies geklärt ist, kann ich das automatische Einschalten aktivieren per

```shell
sudo fastboot oem off-mode-charge 0
...
(bootloader) off_mode_charge disabled
OKAY [  0.021s]
finished. total time: 0.021s
```

Nach dieser Änderung starte ich das Smartphone neu und fahre es direkt im
Anschluss für einen Test herunter. Was passiert, wenn das Telefon nun an ein
Ladegerät angeschlossen wird, zeigt dieses Video

Kaum ist der USB-Stecker drin, startet das Telefon!

Will man den Ursprungzustand wieder herstellen, so geht dies im Bootloader-Modus
über

```shell
sudo fastboot oem off-mode-charge 1
```
