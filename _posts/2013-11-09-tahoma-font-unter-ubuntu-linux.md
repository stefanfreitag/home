---
title: Tahoma-Font unter Ubuntu Linux
date: 2013-11-09T22:57:31+01:00
author: Stefan Freitag
---
So manche Java-Applikation entwickle ich teils unter Microsoft Windows, teils
unter Ubuntu Linux. Damit die graphische Oberfläche unter beiden
Betriebssystemen identisch ausschaut, sollten unter anderen die verwendeten
Fonts auf beiden Systemen verfügbar sein. Um die Font Tahoma auf meinem Ubuntu
Linux verfügbar zu machen, war ein wenig Aufwand erforderlich, da diese nicht im
Paket `ttf-mscorefonts-installer` enthalten ist.

- Die Font ist in der Cabinet-Datei `IELPKTH.CAB` enthalten. Die Datei ist an unterschiedlichen Stellen im Netz zu finden.
  
  ```shell
  wget http://download.microsoft.com/download/ie6sp1/finrel/6_sp1/W98NT42KMeXP/EN-US/IELPKTH.CAB
  ```

- Die Font wird per `cabextract` extrahiert.

  ```shell
  cabextract -F 'tahoma*ttf' IELPKTH.CAB
  ```

- Die extrahierte Font in das Font-Verzeichnis verschieben und die
  Zugriffsberechtigungen setzen.

  ```shell
  mv -f tahoma*ttf /usr/share/fonts/truetype/msttcorefonts/
  chmod 644 /usr/share/fonts/truetype/msttcorefonts/tahoma*
  ```

- Den Font-Cache aktualisieren.

   ```shell
   fc-cache
   ```

- Entfernen der heruntergeladenen Cabinet-Datei.

   ```shell
   rm -f IELPKTH.CAB
   ```
