---
title: Erzeugen einer PuTTY Private Key-Datei
author: Stefan Freitag
date: 2012-08-09T20:50:31+02:00
layout: post
---

Mit dem teilweisen Umstieg von meinem Favorite-Betriebssystem auf Microsoft
Windows stand auch der Einsatz eines Secure Shell-Clients für MS Windows ins
Haus. Einer der bekannteren Clients ist dabei [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Damit der PuTTY Client mit meinem auf dem Secure Shell-Server hinterlegten
öffentlichen Schlüssel zusammenarbeitet, bedarf es einer so genannten ppk-Datei.
Diese Datei wird anhand des privaten Teils des Secure Shell-Schlüsselpaares
mittels PuTTYgen erzeugt.  Hier die Einzelschritte zur Erzeugung der ppk-Datei:

- Das Menü _Conversions_ öffnen und den Punkt _Import key_ selektieren
- Im folgenden Dialog den privaten Teil des SSH-Schlüsselpaars
  (meist eine Datei namens _id_rsa_) selektieren und mit _Open_ bestätigen.
- Sollte der private Teil mit einem Passwort gesichert sein, so ist dieses im
  Folgedialog anzugeben. Bestätigen mit einem Klick auf _OK_.
- Danach kann die ppk-Datei durch Auswahl von _Save private key_ erzeugt werden.

Nun liegt die ppk-Datei im Dateisystem vor und kann in PuTTY eingebunden werden.
