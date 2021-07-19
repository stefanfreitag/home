---
title: Der LedBorg ist da!
date: 2013-05-16T21:45:59+02:00
layout: post
---
Wer die letzten Blog-Einträge verfolgt hat, kann erahnen welchen Einsatzzweck
der [LedBorg](https://www.piborg.org/ledborg "LedBorg Homepage") haben wird.
Doch zunächst ein wenig dazu, was der LedBorg überhaupt ist.

Den Kern-Bestandteil des LedBorg bildet die LED ASMT-YTC2-0AA02 von Avago.
Sie befindet sich im obigen Bild links neben dem kleinen PiBorg und kann mit
Rot, Grün und Blau drei Farben unabhängig voneinander darstellen. Für Fans von
Zahlen hier die Eckdaten der LED:

| Kenngröße         | Rot     | Grün    | Blau    |
| ----------------- | ------- | ------- | ------- |
| Wellenlänge       | 629nm   | 521nm   | 464nm   |
| Lichtstärke       | 450 mcd | 560 mcd | 180 mcd |
| Durchlassspannung | 2.1 V   | 3.2 V   | 3.2 V   |
| Durchlassstrom    | 50 mA   | 30 mA   | 30 mA   |

Neben der LED befindet sich noch ein 74ACT08 onboard. Dieser ist auf der
Eingangsseite mit den GPIO-Pins und auf der Ausgangsseite mit der LED
verbunden - soviel zur Hardware, es gibt aber auch Software für den LedBorg!
Diese ermöglicht die Ansteuerung des LedBord per Konsole und graphischer
Oberfläche. Einen Haken hat die Software allerdings: man benötigt eines der
unterstützen Betriebssysteme

- Raspbian
- Raspbmc
- Arch Linux

und muss beim Download auf die Hardware-Revision des Pi achten. Da der
Quellcode frei zugänglich ist, kann die Software auch in Eigenregie
übersetzt werden.
Glücklicherweise läuft auf meinem Pi ein Raspian, so dass ich auf ein
vorgefertigtes Software-Archiv zurückgreifen konnte. Die Schritte zur
Installation sind wie folgt:

```shell
mkdir ~/ledborg-setup
cd ~/ledborg-setup
wget -O setup.zip http://www.piborg.org/downloads/ledborg/raspbian-2012-10-28-rev2.zip
unzip setup.zip
chmod +x install.sh
./install.sh
```

Die vorletzte Zeile des Skripts `install.sh` sorgt dafür, dass die LED grün
aufleuchtet. Gleichzeitig gibt sie Aufschluss über die Ansteuerung der LED
per Kommandozeile:

```shell
echo "010" > /dev/ledborg
```

Die drei Ziffern entsprechen den Helligkeiten der Farben Rot, Grün und Blau.
Gültige Werte sind 0 (aus), 1 (50% Duty Cycle) und 2 (100% Duty Cycle).
Zum Abschluss noch eine Auflistung von RGB-Werten für einige Farben:

| Farbe          | Rot | Grün | Blau |
| -------------- | --- | ---- | ---- |
| Rot            | 2   |      |      |
| Orange         | 2   | 1    |      |
| Magenta        | 2   |      | 2    |
| Azur           |     | 1    | 2    |
| Gelb           | 2   | 2    |      |
| Weiß           | 2   | 2    | 2    |
| Pseudo-Schwarz |     |      |      |
