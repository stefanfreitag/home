---
title: Hinzufügen eines Ubuntu PPA
date: 2014-11-19T20:45:21+01:00
author: Stefan Freitag
layout: post
---

Für tilestream wird eine spezielle Version der Software [node.js](http://nodejs.org/) empfohlen, die über ein separates Ubuntu Repository, ein _Personal Package Archive_ (PPA), bezogen werden kann.

Ein PPA kann recht einfach in die vorhandene Ubuntu Distribution eingebunden werden: es wird (alternativ zum direkten Editieren der Datei `/etc/apt/sources.list`) lediglich das Tool `/usr/bin/add-apt-repository` benötigt. Dieses liegt im Paket `software-properties-common`, welches ggf. installiert werden muss.

Der allgemeine Aufruf für das Hinzufügen eines PPA lautet

```shell
sudo add-apt-repository ppa:LP-BENUTZER/PPA-NAME 
```

wobei `LP-BENUTZER` dem [Launchpad](https://launchpad.net/)-Benutzernamen des Repository-Inhabers entspricht. Für das zuvor erwähnte node.js lautet die passende Zeile

```shell
sudo apt-add-repository ppa:chris-lea/node.js</pre>
```

Im Anschluss an diesen Befehl muss noch der Signing Key hinzugefügt werden

```shell
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C7917B12

Executing: gpg --ignore-time-conflict --no-options --no-default-keyring --homedir /tmp/tmp.FB4Hml03sC --no-auto-check-trustdb --trust-model always --keyring /etc/apt/trusted.gpg --primary-keyring /etc/apt/trusted.gpg --keyserver keyserver.ubuntu.com --recv-keys C7917B12
gpg: requesting key C7917B12 from hkp server keyserver.ubuntu.com
gpg: key C7917B12: public key "Launchpad chrislea" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```

Übrig bleibt die Aktualisierung des lokalen Paket-Caches:

```shell
sudo apt-get update
```
