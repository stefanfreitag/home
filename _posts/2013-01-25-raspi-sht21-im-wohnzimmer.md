---

title: Raspi-SHT21 im Wohnzimmer
date: 2013-01-25T22:20:35+01:00
author: Stefan Freitag
tags:
  - raspberry-pi
---
Neben dem zuletzt beschriebenen Raspi-LCD hatte ich mir über den Webshop von
[Emsystech Engineering](http://www.emsystech.de/) auch den Raspi-SHT21 zugelegt.

Der SHT21 an sich ist ein digitaler Feuchte- und Temperatursensor und überrascht
durch sein kompaktes Auftreten. Alle notwendigen Komponenten sind auf einer
Fläche von 3 x 3 mm untergebracht - whow! Hier eine Übersicht über die
wichtigsten Kenngrößen des Bausteins:

- Schnittstelle: I2C-Bus
- Energieaufnahme: 3.2µW (bei 8 bit, 1 Messung pro Sekunde)
- Messbereich (RH): 0 bis 100% relativer Feuchte
- Messbereich (T): -40 bis +125°C (-40 bis +257°F)

Mehr Informationen zu dem SHT21 findet man
[hier](https://www.sensirion.com/en/products/digital-humidity-sensors-for-reliable-measurements/humidity-temperature-sensor-sht2x-digital-i2c-accurate/)
bei Sensirion.

Wie angedeutet erfolgt die Kommunikation zwischen Raspberry Pi und dem Sensor
über den I2C-Bus. Die Einrichtung der I2C-Schnittstelle ist [hier](http://www.stefreitag.de/wp/raspberry-pi/aktivierung-der-i2c-schnittstelle/) beschrieben.

Es geht weiter rüber zu dem für Raspi-SHT21 verfügbaren Demo-Code. Den Code kann
man als `zip`-Datei von dieser
[Webseite](http://web.archive.org/web/20170228130240/http://emsystech.de/raspi-sht21/
"Raspi-SHT21 Demo-Code Download") herunterladen und auf dem Raspberry Pi
entpacken.

```shell
unzip Raspi-SHT21-V3_0_0.zip
```

Danach kann man das spätere Executable über den Aufruf von `make` erstellen

```shell
cd Raspi-SHT21-V3_0_0/
cd source
make clean;make
```

Verlief der Kompiliervorgang erfolgreich, steht der Ausführung nichts mehr im
Wege. Nachfolgend eine Beispiel-Ausgabe von heute abend:

```shell
pi@raspberrypi ~/Raspi-SHT21-V3_0_0/source $ ./sht21 
Raspi-SHT21 V3.0.0 by Martin Steppuhn (www.emsystech.de) [Jan 25 2013 20:07:22]
Options:
   S : [20.0 99]
   L : [temperature=20.0][humidity=99]
   C : [Temperature,20,0][Humidity,99]
RaspberryHwRevision=2
0       20.8    31
1       20.8    31
2       20.8    31
3       20.8    31
4       20.8    31
5       20.8    31
6       20.9    31
```

So misst das kleine Gerät nun vor sich hin... bei mir im Wohnzimmer. Als
nächstes werde ich die Visualisierung der Daten per Web-Oberfläche ausprobieren.
Der heruntergeladene Demo-Code enthält fast schon alle dafür notwendigen
Komponenten.