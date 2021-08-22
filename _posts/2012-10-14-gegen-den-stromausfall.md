---
title: 'Gegen den Stromausfall'
date: 2012-10-14T09:55:39+02:00
author: Stefan Freitag
---

Für alle, die es letzten Freitag nicht mitbekommen haben: es gab gegen 13.20 Uhr
einen kurzfristigen Spannungseinbruch durch ein Problem in der Umspannanlage
Derne. Der Spannungseinbruch hatte zur Folge, dass einige Systeme wie die
Rolltreppen in der Thier-Galerie ihre Tätigkeit einstellten.

Bemerkbar machte sich der Spannungseinbruch für mich selbst, indem das Licht im
Büro kurz flackerte und einige von mir überwachte Rechner nicht mehr erreichbar
waren. Da es gleich eine Menge an Rechnern aus dem gleichen Segment waren, lag
der Verdacht nah, dass sich dort einfach ein Switch/ Router aufgehangen hat oder
gerade dabei ist sich neu zu starten.

Dem Rest der IT-Infrastruktur ist aber weiter nichts passiert. Dies liegt u.a.
auch daran, dass die Kernkomponenten an eine USV, eine Unterbrechungsfreie
Stromversorgung (nichts anderes als eine große Batterie), angeschlossen sind. Je
nach Kapazität der USV kann diese für weitere 5 bis 15 Minuten Strom liefern, so
dass genug Zeit bleibt, auf den Stromausfall zu reagieren. Letzteres bedeutet
fast immer, dass man die Rechner ordentlich runterfährt, um Datenverluste zu
vermeiden.

Bei den USVs ist für jeden Geldbeutel etwas dabei und wie immer sind nach oben
keine Grenzen gesetzt. Einen besonderen Blick sollte man allerdings auf die
vorhandenen Anschlußmöglichkeiten werfen:

- Wie viele Endgeräte sind anschließbar?
- Wie ist die USV ansprechbar?

Auf die zweite Frage findet man Antworten wie RS232, USB und Webserver, falls
die USV eine Netzwerkschnittstelle unterstützt. Für den direkten Einsatz mittels
RS232 und USB gibt es allerdings auch gute Werkzeuge in der
OpenSource-Community, z.B. das [Network UPS
Tool](http://www.networkupstools.org/), kurz NUT. Dieses unterstützt über
verschiedene &#8222;Driver&#8220; eine ganze Reihe von USV (eine vollständige
Auflistung ist in der [Hardware Compatibility
List](http://www.networkupstools.org/stable-hcl.html) nachzulesen) und lässt
sich sehr gut in das Monitoring-Werkzeug [Icinga](https://www.icinga.org/)
integrieren.

NUT ermöglicht etwa die Anfrage des USV-Status per CLI. Nachfolgend als Beispiel
eine gekürzte Ausgabe eines solchen Aufrufs.

```shell
device.type: ups
driver.name: blazer_usb
input.frequency: 50.0
input.frequency.nominal: 50
input.voltage: 225.0
input.voltage.fault: 225.1
input.voltage.nominal: 230
output.voltage: 229.9
ups.load: 26
ups.mfr: 
ups.status: OL
ups.temperature: 25.0
ups.type: online
```

Neben den Spannungswerten und Frequenzen (Soll/Ist) ist im unteren Bereich auch
die aktuelle Auslastung der USV (26%) und die interne Temperatur abzulesen.

Bevor ich es vergesse, die meisten USVs lassen sich auch fernsteuern. Aber dazu
ein anderes Mal mehr.
