---
title: Tor!!! Tor!!! Tor!!!
date: 2012-11-17T13:43:11+01:00
author: Stefan Freitag
---
Nach meinem letzten Artikel zum LesArt.Festival liegt bei dem Titel der Schluss
zum BVB und dem Fußball nah. Doch handelt es sich bei diesem Tor um eine
Möglichkeit seine Spuren im Internet - zumindest etwas - zu verwischen.

Tor entspricht in diesem Kontext einem internationalen Netzwerk zur
Anonymisierung von Verbindungsdaten. Um dieses Netzwerk für sich selbst nutzen
zu können, muss auf dem lokalen Rechner ein Tor-Client installiert sein. Auf
meinem aktuellen Ubuntu Linux ging das wie folgt:

```shell
sudo apt-get install tor
```

Während der Installation wird Tor automatisch so eingerichtet, dass es nach dem
Neustart des Systems zur Verfügung steht. Wer den aktuellen Status des Dienstes
prüfen mag, nutzt einfach

```shell
sudo service tor status
```

Was allerdings noch fehlt, ist die Integration in den Web Browser. Dazu
verwende ich mit `polipo` einen kleinen, lokalen Web Proxy, der später im
Web Browser eingetragen wird.

```shell
sudo apt-get install polipo
```

In der Datei `/etc/polipo/conf` sind folgende Werte anzupassen:

```shell
socksParentProxy=localhost:9050   # Lokaler Tor
diskCacheRoot=""                  # Cache abschalten
disableLocalInterface=true        #
```

Mein Web Browser der Wahl ist derzeit Chromium. Um ein wenig die Übersicht zu
behalten, wird Kollege Firefox dediziert über den Web Proxy und Tor laufen, und
Chromium ohne Tor ins Netz gehen. In Firefox wird daher unter dem Punkt Netzwerk
die Adresse des lokalen polipo Web Proxy (in meinem Fall: 127.0.0.1, Port 8123)
eingetragen. That is it 🙂

Man sollte aber trotzdem überprüfen, ob die Konfiguration erfolgreich war. Das
geht beispielsweise über den Aufruf dieser
[URL](https://check.torproject.org/?lang=de).

Mit Tor-ARM (anonymizing relay monitor) gibt es auch ein kleines Werkzeug,
welches einen Einblick in den lokalen Tor-Client gibt. Installiert wird es per

```shell
sudo apt-get install tor-arm
```

Damit sich ARM mit Tor unterhalten kann, muss in der Tor-Konfiguration der
ControlPort aktiviert werden. Danach ist ein Neustart von Tor erforderlich.
