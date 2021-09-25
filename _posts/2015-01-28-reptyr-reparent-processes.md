---
title: 'reptyr - Reparent processes'
date: 2015-01-28T20:31:56+01:00
author: Stefan Freitag
---

[reptyr](https://github.com/nelhage/reptyr) ist mir bereits vor circa zwei
Jahren einmal über den Weg gelaufen und vor Kurzem hätte ich es erneut gut
gebrauchen können - mir war allerdings der Name entfallen. Dieser Blog-Eintrag
dient mir so unter anderem als Gedächtnisstütze 🙂

Das typische reptyr-Szenario besteht aus der Kombination von vergesslichen
Nutzern (also meine Wenigkeit wie nach der Einleitung unmittelbar klar wird) und
langlaufenden Prozessen auf entfernten Systemen. Typischerweise greift man auf
die entfernten Systeme per [ssh](http://de.wikipedia.org/wiki/Secure_Shell) zu,
startet dort einen Multiplexer wie [screen](http://www.gnu.org/software/screen/)
oder [tmux](https://tmux.github.io/) und erledigt darin die durchzuführenden
Arbeiten.  
Unterlässt man allerdings den zweiten Schritt, so führt das Schließen der
ssh-Verbindung auch zum Abbruch der in der Shell ausgeführten Arbeiten - also
zum Abbruch der langlaufenden Prozesse. Das Fehlen des Terminal Multiplexers
fällt einem spätestens dann ein, wenn man sich am entfernten System abmelden
möchte. An diesem Punkt angekommen, sind die long running jobs schon gestartet -
stellt sich also die Frage, wie man diese am Leben erhält. reptyr bietet hierauf
eine Antwort.

In der Ubuntu Linux-Distribution lässt es sich wie folgt installieren:

```shell
$ sudo apt-get install reptyr

Paketlisten werden gelesen... Fertig
Abhängigkeitsbaum wird aufgebaut.
Statusinformationen werden eingelesen.... Fertig
Die folgenden NEUEN Pakete werden installiert:
  reptyr
0 aktualisiert, 1 neu installiert, 0 zu entfernen und 8 nicht aktualisiert.
Es müssen 19,6 kB an Archiven heruntergeladen werden.
Nach dieser Operation werden 81,9 kB Plattenplatz zusätzlich benutzt.
Holen: 1 http://de.archive.ubuntu.com/ubuntu/ trusty/universe reptyr amd64 0.5-1 [19,6 kB]
Es wurden 19,6 kB in 0 s geholt (83,0 kB/s).
Vormals nicht ausgewähltes Paket reptyr wird gewählt.
(Lese Datenbank ... 383521 Dateien und Verzeichnisse sind derzeit installiert.)
Vorbereitung zum Entpacken von .../reptyr_0.5-1_amd64.deb ...
Entpacken von reptyr (0.5-1) ...
Trigger für man-db (2.6.7.1-1ubuntu1) werden verarbeitet ...
reptyr (0.5-1) wird eingerichtet ...
```

Ist die Installation abgeschlossen, kann reptyr sofort genutzt werden. Die Einzelschritte und der Aufwand zum Umhängen eines Prozesses in einen Multiplexer sind sehr überschaubar:

- Anhalten des langlaufenden Prozesses mittels `STRG+Z`
- Fortsetzen des Prozesses im Hintergrund mittels `bg`
- Anzeige der im Hintergrund existierenden Prozesse über `jobs -l`, wobei man sich die PID des relevanten Prozesses merkt
- &#8222;Disownen&#8220; des Prozesses anhand seiner PID: `disown PID`
- screen bzw. tmux starten
- Innerhalb des Terminal Multiplexers reptyr mit der PID als Argument starten

That's it! Danach kann man sich vom Multiplexer abnabeln (detachen) und die ssh-Verbindung beenden.
