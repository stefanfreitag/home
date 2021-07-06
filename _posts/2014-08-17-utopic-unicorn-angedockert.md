---
title: 'Utopic Unicorn  &#8211; ange&#8220;docker&#8220;t'
date: 2014-08-17T18:18:56+02:00
author: Stefan Freitag
layout: post
---

Seit meinen jungen Tagen bin ich durch diverse Linux-Distributionen wie [Debian](http://www.debian.org/index.de.html "Debian Homepage"), das jetzige [openSUSE](http://de.opensuse.org/Hauptseite "openSUSE Homepage") und auch [Scientific Linux](https://www.scientificlinux.org/ "Scientific Linux Homepage") gestolpert, um letztendlich bei [Ubuntu](http://www.ubuntu.com/ "Ubuntu Homepage") hängenzubleiben.

Mit jedem Release einer neuen Ubuntu-Version steige ich auch um, dieses Mal möchte ich mir aber im Vorfeld ein Bild vom kommenden Release [Utopic Unicorn](http://wiki.ubuntuusers.de/Utopic_Unicorn "Wiki-Seite zu Utopic Unicorn") machen &#8211; und das natürlich ohne mein aktuell installiertes System zu zerschießen. Als Lösung bietet sich die Installation in einer virtuellen Maschine an, wobei ich statt [VirtualBox](https://www.virtualbox.org/ "VirtualBox Homepage") dieses Mal den Weg über [Docker](http://www.docker.com/ "Docker Homepage") ausprobieren möchte. Die Installation von Docker erweist sich als sehr einfach:

```shell
sudo apt-get install docker.io
```

Ist die Installation durch, kann man sich erstmal allgemeine Informationen zu Docker ausgeben lassen. Bei mir liefert der Aufruf von Docker mit der Option `info` aktuell folgendes:

```shell
sudo docker.io info

Containers: 6
Images: 7
Storage Driver: aufs
 Root Dir: /media/stefan/ab770352-e523-4ada-9afb-dcf6f490b44b/var_lib_docker/aufs
 Dirs: 25
Execution Driver: native-0.1
Kernel Version: 3.13.0-33-generic
WARNING: No swap limit support
```

So weit, so gut. Um endlich mit dem Testen des Utopischen Einhorns beginnen zu können, lade ich das Image herunter per `pull` (das erinnert mich ein wenig an `git`).

```shell
sudo docker.io pull ubuntu:utopic
Pulling repository ubuntu
75204fdb260b: Download complete
511136ea3c5a: Download complete
af82eb377801: Download complete
f33dbb8bc20e: Download complete
92ac38e49c3e: Download complete
aa822e26d727: Download complete
31db3b10873e: Download complete
```

Der Zugriff auf das heruntergeladene Image erfolgt beispielsweise über die (kurze) Image ID. In meinem Fall ist dies die 75204fdb260b wie der Aufruf von Docker mit der Option `images` zeigt:

```shell
sudo docker.io images 

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              utopic              75204fdb260b        5 days ago          230.1 MB
```

Unter Verwendung des Image kann ein Container gestartet werden. Im einfachsten Fall zündet man eine interaktive Session über die Option `i` und gelangt per `t`-Option an ein Pseudo-Terminal:

```shell
sudo docker.io run -t -i 75204fdb260b
root@b992c03bb8fc:/#

sudo docker.io run -t -i ubuntu:utopic
root@a381af6880bf:/#
```

An obigem Beispiel ist erkennbar, dass man das Image über seine kurze ID als auch über `ubuntu:utopic` ansprechen kann. Mit einem einfachen `exit` verlässt man den Container übrigens wieder.  
Insgesamt läuft damit das Einhorn erstmal eingezäunt in einem Container und wird in den kommenden Tagen von mir genauer untersucht werden.
