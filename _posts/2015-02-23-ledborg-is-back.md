---
title: LedBorg is back
date: 2015-02-23T21:30:01+01:00
author: Stefan Freitag
---

Beim Durchstöbern meines Bastelkrams kam ein alter Freund, der
[LedBorg](https://www.piborg.org/ledborg), zum Vorschein. Mit ihm hatte ich
bereits ein oder zwei kleinere Projekte realisiert. Eine Ansteuerung aus Java
heraus fehlte allerdings noch. Glücklicherweise lässt sich diese Ansteuerung
sehr einfach mittels der [Pi4J](http://pi4j.com/)-Bibliothek umsetzen.

- Als erstes erzeuge ich eine Pin-Konfiguration, um die drei Farbkanäle Rot,
  Grün und Blau der Tri-Colour LED separat ansteuern zu können. Die zu
  verwendenden Pins kann man den [Specs](https://www.piborg.org/ledborg/specs
  "Specs zum LedBorg") zum LedBorg entnehmen.
  
  ```java
  PinLayout LAYOUT =
  PinLayout.create(RaspiPin.GPIO_00, RaspiPin.GPIO_02, RaspiPin.GPIO_03);
  ```

- Die Farbintensität auf den einzelnen Kanälen lässt sich per
  [PWM](http://de.wikipedia.org/wiki/Pulsweitenmodulation "Wikipedia-Seite zur
  Pulsweitenmodulation") regeln. Jeder Kanal wird mit 0% duty cycle
  initialisiert und der Maximalwert lege ich auf 50% duty cycle.
  
  ```java
  SoftPwm.softPwmCreate(pinLayout.getRed().getAddress(), 0, 50);
  SoftPwm.softPwmCreate(pinLayout.getGreen().getAddress(), 0, 50);
  SoftPwm.softPwmCreate(pinLayout.getBlue().getAddress(), 0, 50);
  ```

- Soll die LED eine Farbe anzeigen, so muss diese in % duty cycles pro Kanal
  umgerechnet werden. Dazu wird die Farbe per `getRGBColorComponents` in drei
  float-Werte von 0.0 bis 1.0 zerlegt und dann jeweils mit dem Maximalwert von
  50 skaliert.

  ```java
  final float[] colors = color.getRGBColorComponents(null);

  SoftPwm.softPwmWrite(layout.getRed().getAddress(), (int) (colors[0] * 50f));
  SoftPwm.softPwmWrite(layout.getGreen().getAddress(), (int) (colors[1] * 50f));
  SoftPwm.softPwmWrite(layout.getBlue().getAddress(), (int) (colors[2] * 50f));
   ```

Soviel zu den Trockenübungen: auf
[GitHub](https://github.com/stefanfreitag/LedBorg/) habe ich ein Repository mit
dem aktuellen Quellcode und einer Demo-Applikation hinterlegt.
