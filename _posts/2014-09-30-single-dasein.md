---
title: Single-Dasein
date: 2014-09-30T20:55:14+02:00
author: Stefan Freitag
---

Puuuh, das ist echt nicht einfach - also das Programmieren meine ich. Nun hat
mich ein Design Pattern, genauer dessen Umsetzung in multithreaded Anwendungen,
kalt erwischt.   Bei dem Pattern handelt es sich um das allseits bekannte
[Singleton](http://de.wikipedia.org/wiki/Singleton_(Entwurfsmuster)
"Wikipedia-Seite zu Singleton"). Das Pattern ist leicht zu verstehen und kommt
dort zum Einsatz, wo

- sichergestellt werden soll, dass das Objekt nur einmal instanziiert werden kann
- ein globaler Zugang zu dem Objekt existieren soll

Im Zusammenhang mit dem Schlagwort der lazy initialization (die
Singleton-Instanz wird erst beim ersten Zugriff initialisiert), bin ich bei
folgendem Code gelandet:

```java
public class Singleton {

    private static Singleton instance = null;
 
    private Singleton() {}
 
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new ClassicSingleton();
        }
        return instance;
    }
}
```

Für single-threaded Anwendungen ist der Ansatz gut geeignet. Kommen aber mehr
Threads ins Spiel, die unabhängig voneinander in der Methode `getInstance()`
landen, gibt es Probleme: Hat der erste Thread die Zuweisung `instance = new
Singleton()` erreicht aber noch nicht ausgeführt, und tritt der zweite parallel
in die Methode ein und wertet die Bedingung `if (instance == null)` als wahr
aus, dann erzeugt jeder der beiden Threads eine Instanz. Zeitgleich wäre es
damit um das hinter dem Singleton stehende Konzept geschehen.

Der erste Gedanke, um das Singleton zu erweitern, geht natürlich in Richtung
synchronisierter Zugriff. Hier gibt es auch verschiedene Lösungsansätze, auf die
ich aber nicht weiter eingehen möchte. Stattdessen bin ich auf das so genannte
[Initialization-on-demand\_holder\_idiom](http://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom
"Wikipediaseite zu Initialization-demand holder idiom") gestoßen. Die damit
aufgebaute Singleton hat folgende Struktur:

```java
public class Singleton {

    private Singleton() {}
 
    private static class LazyHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
 
    public static Singleton getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```

Nun muss nur noch überlegt werden, warum diese Struktur hilfreich sein soll.
Dazu eine Betrachtung des zeitlichen Ablaufs beim Zugriff auf das Singleton:

- Wird die Singleton-Klasse das erste Mal durch die Java Virtual Machine
  geladen, so durchläuft sie einen Initialisierungsprozess, bei dem etwa die
  statischen Felder initialisiert werden. Da hier keine solchen Felder
  existieren, passiert im Großen und Ganzen nichts. Insbesondere wird die
  innenliegende statische Klasse _nicht_ initialisiert.
- Die innenliegende Klasse wird erst beim ersten Aufruf der statischen Methode
  `getInstance()` durch die JVM geladen und initialisiert. Nun greift die
  vorherige Bemerkung bezüglich der Initialisierung statischer Felder: während
  der Initialisierung von `LazyHolder` wird der private Konstruktor des
  Singletons aufgerufen und das Ergebnis `INSTANCE` zugewiesen.
- Der Clou an der Sache ist nun, dass für die Initialisierung der inneren Klasse
  Java selbst gemäß der
  [JLS](http://docs.oracle.com/javase/specs/jls/se8/html/jls-12.html#jls-12.4.2
  "Link zur JLS (Detaillierte Initialisierungsprozedur für Klassen)") die
  Synchronisation übernimmt. Damit wäre also eine Synchronisation in dem Code
  verbaut, ohne ein explizites `synchronized` zu verwenden. Gefällt mir!
  