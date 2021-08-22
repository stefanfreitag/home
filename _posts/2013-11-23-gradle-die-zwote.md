---
title: Gradle, die Zwote
date: 2013-11-23T17:54:07+01:00
author: Stefan Freitag
tags:
  - gradle
---
Zuletzt hatte ich in
[diesem Blog-Eintrag](2013-11-08-hello-world-java-the-gradle-way.md) beschrieben
wie man ein einfaches Java-Projekt mit [Gradle](http://www.gradle.org/) bauen
kann. Dieses Beispiel wird nun etwas erweitert.

Es geht manchmal nicht nur um das Bauen bzw. Ausführen eines Projekts, sondern
auch um dessen Verteilung. Typischerweise findet man draußen in der Welt
Projekte, die Quellcode, Dokumentation und die compilierte Software in separaten
JARs zum Download anbieten. Die Erzeugung genau dieser Struktur wird für das
"Hello World, Java"-Projekt mit dem folgenden Gradle Build-Skript erreicht:

```plain
project.description = 'Hello World'
project.version = '1.0'

apply plugin: 'java'
apply plugin: 'application'

mainClassName = 'HelloWorld'

task sourcesJar(type: Jar, dependsOn: classes) {
    classifier = 'sources'
    from sourceSets.main.allSource
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}

artifacts {
    archives sourcesJar
    archives javadocJar
}
```

Im Vergleich zur ersten Version des Build-Skripts sind die Zeilen 9 bis 22
hinzukommen.

- Der Task `sourcesJar` wird aufgrund des `dependsOn` nach der Erstellung der
  `Class`-Dateien ausgeführt. Das dabei erstellte JAR verwendet alle
  Quelldateien des Projekts und der Name des Archivs wird durch den Classifier
  zu `HelloWorld-1.0-sources.jar`.

  ```plain
  task sourcesJar(type: Jar, dependsOn: classes) {
      classifier = 'sources'
      from sourceSets.main.allSource
  }
  ```

- Nach der eben gegebenen Erklärung sollte der Task `javadocJar` leicht zu
  verstehen sein. Aufgrund des `dependsOn` wird der Task nach der Erstellung
  der Javadoc-Dateien ausgeführt. Das dabei erstellte JAR verwendet alle Dateien
  aus dem Javadoc-Zielverzeichnis. Der Name des Archivs wird durch den
  Classifier zu `HelloWorld-1.0-javadoc.jar`.

  ```plain
  task javadocJar(type: Jar, dependsOn: javadoc) {
      classifier = 'javadoc'
      from javadoc.destinationDir
  }
  ```

- Als Artefakte werden solche Dateien bezeichnet, die das Projekt für die
  Außenwelt bereitstellt. Damit ist klar, warum im Artefakt-Abschnitt die
  beiden Tasks erscheinen.

  ```plain
  artifacts {
      archives sourcesJar
      archives javadocJar
  }
  ```

Ist das Build-Skript erweitert, kann es ausgeführt werden. Wie man sieht,
erscheinen die beiden neu definierten Tasks in der Auflistung der
ausgeführten Tasks:

```shell
gradle -b build.gradle clean build
:clean
:compileJava
:processResources UP-TO-DATE
:classes
:jar
:javadoc
:javadocJar
:sourcesJar
:assemble
:compileTestJava UP-TO-DATE
:processTestResources UP-TO-DATE
:testClasses UP-TO-DATE
:test UP-TO-DATE
:check UP-TO-DATE
:build

BUILD SUCCESSFUL

Total time: 9.436 secs
```