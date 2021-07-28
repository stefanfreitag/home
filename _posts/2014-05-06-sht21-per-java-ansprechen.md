---
title: SHT21 per Java ansprechen
date: 2014-05-06T22:04:17+02:00
author: Stefan Freitag
layout: post
---

Nach dem RaspiLCD habe ich mir den Temperatur- und Luftfeuchtigkeitssensor SHT21
vorgenommen und dafür mit Hilfe von pi4j eine Java-Bibliothek erstellt. Den
ganzen Code gibt es später hier im Blog, zuvor erstmal ein Snippet aus der
Demo-Anwendung. Diese misst alle 5 Sekunden die Temperatur und gibt den Wert in
eine log-Datei (+ Konsole) aus.

```shell
$ sudo java -cp .:pi4j-core-0.0.5.jar:log4j-1.2.17.jar demo.TemperatureDemo 
2014-05-06 19:55:58,695 INFO (main) [de.freitag.stefan.sht21.SHT21Impl] - Connecting to bus 1
2014-05-06 19:55:59,001 INFO (main) [de.freitag.stefan.sht21.SHT21Impl] - Connecting to device with address 40
2014-05-06 19:55:59,028 DEBUG (main) [de.freitag.stefan.sht21.SHT21Impl] - Starting temperature measurement
2014-05-06 19:55:59,367 INFO (main) [demo.TemperatureDemo] - Measured temperature [deg C]: 22.198181
2014-05-06 19:56:04,369 DEBUG (main) [de.freitag.stefan.sht21.SHT21Impl] - Starting temperature measurement
2014-05-06 19:56:04,507 INFO (main) [demo.TemperatureDemo] - Measured temperature [deg C]: 22.187454
2014-05-06 19:56:09,509 DEBUG (main) [de.freitag.stefan.sht21.SHT21Impl] - Starting temperature measurement
2014-05-06 19:56:09,646 INFO (main) [demo.TemperatureDemo] - Measured temperature [deg C]: 22.198181
2014-05-06 19:56:14,649 DEBUG (main) [de.freitag.stefan.sht21.SHT21Impl] - Starting temperature measurement
2014-05-06 19:56:14,786 INFO (main) [demo.TemperatureDemo] - Measured temperature [deg C]: 22.187454
2014-05-06 19:56:19,788 DEBUG (main) [de.freitag.stefan.sht21.SHT21Impl] - Starting temperature measurement
2014-05-06 19:56:19,926 INFO (main) [demo.TemperatureDemo] - Measured temperature [deg C]: 22.187454
2014-05-06 19:56:24,928 DEBUG (main) [de.freitag.stefan.sht21.SHT21Impl] - Starting temperature measurement
2014-05-06 19:56:25,066 INFO (main) [demo.TemperatureDemo] - Measured temperature [deg C]: 22.187454
```
