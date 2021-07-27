---
title: Fernsehen unter Linux
date: 2013-02-17T13:23:23+01:00
layout: post
---
Die öffentlich-rechtlichen als auch die privaten Fernsehsender stellen seit
geraumer Zeit bereits ausgestrahlte Sendungen und Spielfilme in Mediatheken
bereit. Das es davon eine ganze Menge gibt, sieht man an der folgenden
(unvollständigen) Aufzählung:

- [3sat Mediathek](http://www.3sat.de/mediathek/index.php?display=1&#038;mode=aktuell)
- [ARD Mediathek](http://www.ardmediathek.de/ard/servlet/)
- [ARTE Mediathek](http://www.arte.tv/de "ARTE Mediathek")
- [NDR Mediathek](http://www.ndr.de/mediathek/ "NDR Mediathek")
- [WDR Mediathek](http://www.wdr.de/mediathek/html/regional/index.xml)
- [ZDF Mediathek](http://www.zdf.de/ZDFmediathek)
- [N24 Mediencenter](http://www.n24.de/mediathek/ "N24 Mediathek")
- [Pro7 Mediathek](http://www.prosieben.de/video/)
- [RTL Mediathek](http://rtl-now.rtl.de/ "RTL Mediathek")
- [Sat.1 Mediathek](http://www.sat1.de/video)
- [VOX Mediathek](http://www.voxnow.de/ "VOX Mediathek")

ARD und ZDF streamen mittlerweile auch das aktuelle TV-Programm ins Netz, so
dass man sich mit einem `rtsp`-fähigen Client direkt dranhängen kann. Ein
solcher Client ist der `vlc`, der zum Beispiel über

```shell
vlc rtsp://3gp-livestreaming1.zdf.de/liveedge2/de10_v1_710.sdp
```

das Programm des ZDF abspielt. Die Endpunkte der anderen Sender kurz im
Überblick:

- ARD: `rtsp://daserste.edges.wowza.gl-systemhaus.de/live/mp4:daserste_int_576`
- ZDF.neo: `rtsp://3gp-livestreaming1.zdf.de/liveedge2/de09_v1_710.sdp`
- ZDF.kultur: `rtsp://3gp-livestreaming1.zdf.de/liveedge2/de07_v1_710.sdp`
- ZDF.info: `rtsp://3gp-livestreaming1.zdf.de/liveedge2/de08_v1_710.sdp`

wobei die Qualität noch zu wünschen übrig lässt. Damit sich die TV-Kanäle zügig
über die Kommandozeile starten lassen, kann man Aliase einrichten.

```shell
alias ard='vlc rtsp://daserste.edges.wowza.gl-systemhaus.de/live/mp4:daserste_int_576'
alias zdf='vlc rtsp://3gp-livestreaming1.zdf.de/liveedge2/de10_v1_710.sdp'
alias zdfneo='vlc rtsp://3gp-livestreaming1.zdf.de/liveedge2/de09_v1_710.sdp'
alias zdfkultur='vlc rtsp://3gp-livestreaming1.zdf.de/liveedge2/de07_v1_710.sdp'
alias zdfinfo='vlc rtsp://3gp-livestreaming1.zdf.de/liveedge2/de08_v1_710.sdp'
```
