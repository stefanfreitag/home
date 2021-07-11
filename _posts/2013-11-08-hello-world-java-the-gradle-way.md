---
title: “Hello World, Java!” – The Gradle Way
date: 2013-11-08T23:03:50+01:00
author: Stefan Freitag
layout: post
---

In diesem [Blog-Eintrag](http://www.stefreitag.de/wp/2013/11/06/hello-world-java-ant-style/)
hab ich mich ein wenig an die guten alten Zeiten, zu denen ich Ant-Skripte noch
von Hand zusammengeschrieben habe, erinnert. Allerdings muss ich aus heutiger
Sicht sagen: Gut, dass die alten Zeiten vorbei sind! Mittlerweile gibt es neue
Build-Werkzeuge für Java, die einem das Leben noch einfacher und vor allem auch
bequemer machen. Mir gefällt derzeit [Gradle](http://www.gradle.org/) sehr gut.
Gradle setzt auf die mit [Maven](http://maven.apache.org/) eingeführte Idee
"convention over configuration". Dabei wird unter anderem das Verzeichnislayout
für den Quellcode sowie die Ressourcen fest vorgegeben. Mir ist dies nur recht,
da mein Fokus auf Software-Entwicklung liegt - ich will nicht viel Zeit und
Gedanken auf Build-Skripte & Co verwenden, sondern jemanden haben, der die
Arbeit für mich übernimmt. Wie so etwas aussehen kann, wird am Beispiel
"HelloWorld, Java!" gezeigt. Der Quellcode zur Applikation wird im Verzeichnis
`src/main/java/` (Konvention) abgelegt und die Build-Datei heißt üblicherweise
`build.gradle`. Um den Quellcode zu kompilieren, auszuführen und auch noch ein
Software-Archiv für die spätere Distribution zu bauen, reicht folgendes
Build-Skript:

```plain
project.description = 'Hello World'
project.version = '1.0'

apply plugin: 'java'
apply plugin: 'application'

mainClassName = 'HelloWorld'
```

Jepp, das ist kurz und knackig! Über das Plugin `java` werden
(um in Ant-Speak zu bleiben) Tasks wie etwa `clean` und `jar` eingebunden. Das
zweite Plugin erlaubt die Erstellung eines Software-Archivs. Was in beiden
Fällen auffällt: ich muss weder Quell- noch Zielverzeichnisse für Java-Code
bzw. Java-Klassen etc. angeben. Ist das Build-Skript fertiggestellt, erfolgt
der Aufruf durch die Angabe der auszuführenden Tasks:

```plain
gradle clean distZip run
:clean
:compileJava
:processResources UP-TO-DATE
:classes
:jar
:startScripts
:distZip
:run
Hello World, Java!

BUILD SUCCESSFUL

Total time: 8.746 secs
```
