---
title: Swap-Speicher in Linux-basierten virtuellen Maschinen
date: 2012-08-02T20:52:06+02:00
author: Stefan Freitag
tags: swap linux virtual machine vm
layout: post
---

Sollte auf einem Rechner der physische Speicher knapp werden, greifen das darauf
ausgeführte Betriebssystem und auch die Anwendungen gerne zu dem unter dem
Deckmantel des virtuellen Speichers versteckten Swap-Speicher. Bei virtuellen
Maschinen ist dies nicht anders. Daher wird in deren Ressourcenkonfiguration
neben dem Hauptspeicher meist eine eigene Partition (Image, Logical Volume, ...)
für den Swap-Speicher eingebunden.

Läuft die virtuelle Maschine allerdings bereits im Produktivbetrieb und das
Einbinden einer solchen Partition wurde vergessen, so kann der Swap-Speicher
durch eine in der virtuellen Maschine liegende Datei bereitgestellt werden.
Man muss bei diesem Vorgehen jedoch mit Performanceeinbußen rechnen.

Im Einzelnen sind zur Umsetzung des Datei-basierten Swap-Speichers folgende
Schritte durchzuführen:

- Innerhalb der virtuellen Maschine per `dd` eine Datei anlegen, hier zum
  Beispiel die Datei `/swapfile` mit einer Größe von 1024 MByte

  ```shell
  dd if=/dev/zero of=/swapfile bs=1024 count=1024k
  ```

- Formatieren der Datei mit dem entsprechenden Dateisystem

  ```shell
  mkswap -f /swapfile
  ```

- Da nicht jeder den ausgelagerten Speicherinhalt lesen können soll, sind
  die Zugriffsrechte auf die Datei restriktiv zu setzen. Des Weiteren soll
  die Datei dem Nutzer `root` gehören.

  ```shell
  chmod 600 /swapfile
  chown root:root /swapfile
  ```

- Aktiviert werden kann der Swap-Speicher durch den `swapon` Befehl

  ```shell
  swapon /swapfile
  ```

- Damit der Swap-Speicher nach dem Neustart der virtuellen Maschine
  automatisch eingebunden wird, ist ein Eintrag in der Datei `/etc/fstab`
  notwendig. Dieser könnte wie folgt aussehen:

  ```shell
  /swap    swap     defaults     0     0
  ```
