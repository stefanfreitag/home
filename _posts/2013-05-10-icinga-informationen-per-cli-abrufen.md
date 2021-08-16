---
title: Icinga-Informationen per CLI abrufen
date: 2013-05-10T19:20:00+02:00
author: Stefan Freitag
---
Schon während meiner Zeit an der [Technischen Universität
Dortmund](http://www.tu-dortmund.de)
durfte ich, beiläufig zum normalen Tagesgeschäft aus Lehre und Forschung,
Rechner administrieren. Neben der Installation und Konfiguration von Hard- und
Software gehört auch die Aufrechterhaltung des Betriebs zu einer der obersten
Pflichten eines Admins.

Üblicherweise nimmt die Anzahl der Rechner und darauf ausgeführten Dienste mit
der Zeit eher zu statt ab, so dass eine kontinuierliche Überwachung nur durch
die Unterstützung von Automation sichergestellt werden kann. Wer sich ein wenig
in Internet nach Monitoring-Lösungen umschaut, der stößt zwangsläufig auf
[Nagios](http://www.nagios.org/) und
[Icinga](https://www.icinga.org/). Beide erlauben die
Benachrichtigung per E-Mail, Pager und/ oder SMS. Ich möchte aber, dass mir mein
Raspberry Pi Bescheid sagt, sobald ein Fehler auftritt. In diesem
[Post](http://www.stefreitag.de/wp/2013/05/08/sprechender-pi/)
habe ich bereits beschrieben, wie man einen Pi zum Sprechen bringt. Nun fehlt
noch der aufzusagende Text, der - wie angedeutet - auf den von
Icinga erhaltenen Informationen zu einzelnen Rechnern und Diensten basiert.

Die Informationen werden dem Administrator unter anderem über eine
Web-Oberfläche präsentiert (siehe Abbildung). Einen alternativen Zugang über die
Kommandozeile (CLI) bietet das Shell-Skript
[nagios_commander.sh](https://github.com/brandoconnor/nagios_commander) von
Brandon O&#8217;Connor.  
Das Skript nutzt die in Icinga vorhandenen CGI-Skripte und parst die daraus
zurückgelieferten Informationen. Bevor das Skript eingesetzt werden kann, sind
darin einige Variablen anzupassen:

```shell
NAG_HOST='' (z.B. www.demo.de/icinga)
USERNAME='' (verwendet für die Icinga-Authentifikation)
PASSWORD='' (verwendet für die Icinga-Authentifikation)
NAG_HTTP_SCHEMA='' (http oder https)
```

Auf einen Stolperstein des Skripts bin ich gestoßen, als ich es mit der
`https`-Schnittstelle einer Icinga-Installation getestet habe. Für `https` ist
der Einsatz eines X.509-basierten Zertifikats notwendig, welches in diesem Fall
leider selbst-signiert war. Dadurch verweigerte das in dem Skript verwendete
`curl` die Zusammenarbeit. Dieses Problem ließ sich durch das Hinzufügen des
Schalters `-k` bei allen `curl` Aufrufen leicht beheben.

Ist diese letzte Hürde genommen, kann das Skript wie nachfolgend gezeigt genutzt
werden, um den Status aller in Icinga registrierten Hosts anzuzeigen:

```shell
./nagios_commander.sh -q list  -h
```

Zudem können Host- und Service-Downtimes abgerufen werden per

```shell
./nagios_commander.sh -q host_downtime
./nagios_commander.sh -q service_downtime
```

wie auch Informationen zu Rechnern, die gewissen Hostgruppen angehören

```shell
./nagios_commander.sh  -q list -H http-servers
```
