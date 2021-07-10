---
title: Installation einer Scientific Linux VM per Kickstart
author: Stefan Freitag
date: 2012-09-29T22:27:53+02:00
layout: post
---

Für die Installation des Betriebssystems in eine virtuelle Maschine gibt es die
verschiedensten Vorgehen. Für Ableger von RedHat Linux, z.B. Scientific Linux
und CentOS, kann man auf das Kickstart-Verfahren zurückgreifen. Bei diesem
Verfahren können die zur Installation notwendigen Informationen in Form einer
Datei an die Installationsroutine übergeben werden, d.h. während der
Installation muss man selbst nicht mehr eingreifen.

Nachfolgend sind die einzelnen Schritte für die Installation von Scientific
Linux 5.5 in eine virtuelle Maschine beschrieben. Die erforderlichen
Informationen werden der Installationsroutine auf einer virtuellen
Floppy-Disk übergeben.

- Erzeugen einer Abbild-Datei in Größe einer Floppy Disk

  ```shell
  dd if=/dev/zero of=sl55-i386.img bs=1440K count=1
  ```

- Anlegen des Dateisystems in der Abbild-Datei

  ```shell
  /sbin/mkfs -F -t ext2 sl55-i386.img
  ```

- Einspielen der Kickstartdatei auf die Floppy Disk

  ```shell
  sudo mount -o loop sl55-i386.img /tmp/floppydisk
  cp -p ks.cfg /tmp/floppydisk
  sudo umount /tmp/floppydisk
  ```

- Erzeugen eines Disk Images für die Installation des Betriebssystems

  ```shell
  qemu-img create SL55.qcow 4G
  ```

- Herunterladen des Kernels und der InitRD aus dem `isolinux`-Verzeichnis
- Starten der Installation

  ```shell
  qemu
    -cdrom images/SL.55.051810.DVD.i386.disc1.iso
    -hda SL55.qcow
    -fda sl55-i386.img
    -m 512
    -localtime
    -kernel vmlinuz
    -initrd initrd.img
    -append ks=floppy
  ```
