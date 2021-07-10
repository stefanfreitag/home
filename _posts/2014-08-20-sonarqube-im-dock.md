---
title: SonarQube im Dock
date: 2014-08-20T20:59:20+02:00
author: Stefan Freitag
tags: sonarqube docker
layout: post
---

Mit dem Programmieren ist es so eine Sache: manchmal gewöhnt man sich Dinge an,
die nicht gut sind. Um so besser, wenn einem dann auf die Finger geklopft wird.
Letzteres übernehmen bei mir Werkzeuge, die den geschriebenen Quellcode anhand
verschiedener Regel und Metriken analysieren und Berichte erstellen. Eines
meiner Lieblingswerkzeuge ist Sonar bzw. [SonarQube](http://www.sonarqube.org/ "SonarQube Homepage") wie es heute heißt. SonarQube besteht aus insgesamt drei Komponenten:

- einem Modul für die Integration in Build-Management-Tools wie [Ant](http://ant.apache.org/ "Apache Ant"), [Maven](http://maven.apache.org/ "Maven Homepage") oder [Gradle](http://www.gradle.org/ "Gradle Homepage")
- einer Datenbank zur Persistierung der Testergebnisse
- einem Webserver zur Verwaltung bzw. Visualisierung der Testergebnisse

Über die [GitHub-Seite von Tiago Pires](https://github.com/tpires "GitHub-Seite von Tiago Pires") bin ich auf [docker-sonar](https://github.com/tpires/docker-sonar "Link zu docker-sonar") gestossen, welches seit einigen Tagen auf meinem Rechner in Form von zwei Containern läuft.  
Gemäß der Anleitung von Tiago habe ich zunächst die beiden Images
heruntergeladen: eines enthält die für SonarQube notwendig Datenbank
(hier: MySQL) und das andere den Webserver.

```shell
sudo docker.io pull tpires/sonar-server

Pulling repository tpires/sonar-server
0c8b002d5e81: Download complete 
1fe4551ae843: Download complete
8dbd9e392a96: Download complete
511136ea3c5a: Download complete
5e66087f3ffe: Download complete
5332fecd94d7: Download complete
1317281dd6cf: Download complete
59fbad0fda08: Download complete
db590527a4a8: Download complete
0678282c03c3: Download complete
4d26dd3ebc1c: Download complete
d4010efcfd86: Download complete
99ec81b80c55: Download complete 
b261bc65cd23: Download complete
42404685406e: Download complete
6cc69450fe19: Download complete
efc4fbcd007f: Download complete
2baeb2edbf92: Download complete
ecd5c1cc18ac: Download complete
1f089cc15e82: Download complete
dd92aa709674: Download complete
17e976d16815: Download complete
d69b2024ae1f: Download complete
f9793c257930: Download complete
2f38dcd7f3f4: Download complete
fcacd157cd81: Download complete
0a62e1030a6b: Download complete
3b25b3734551: Download complete
7c0ddaf96d3a: Download complete
ac574cd0af15: Download complete
5df78bb59acb: Download complete
a2e9fc1d6ace: Download complete
755ff74e2de1: Download complete
936b0a0e7ab9: Download complete
adce230f7b96: Download complete
de79bc7778b8: Download complete</pre>
```

```shell
sudo docker.io pull tpires/sonar-mysql
[sudo] password for stefan:
Pulling repository tpires/sonar-mysql
348f1ba32d0d: Download complete
bbcf5a8adec8: Download complete
8dbd9e392a96: Download complete
17e47fa04f24: Download complete
6837c2fb4e8b: Download complete
ddb842ad1c18: Download complete
8de2d7e441a3: Download complete
a437b2651ed7: Download complete
80ee21f9c929: Download complete
680cf1fd2dae: Download complete
b7f010585e63: Download complete
0ec528b984f2: Download complete
8ccf66ccc334: Download complete
511136ea3c5a: Download complete
9bad880da3d2: Download complete
25f11f5fb0cb: Download complete
ebc34468f71d: Download complete
2318d26665ef: Download complete
ba5877dc9bec: Download complete
308cbebd2bfd: Download complete
6e1e835e7e81: Download complete
8e253819e7bd: Download complete
84580fb32fc2: Download complete
76c06922f402: Download complete
a2bcf7a7b14e: Download complete
667c9cb093be: Download complete
ec94d4291222: Download complete
4f224c8894e3: Download complete
```

Nach dem Download geht es an den Start des ersten Containers, der die Datenbank
bereitstellen wird:

```shell
$ sudo docker.io run -i -t -d -p 3306:3306 --name smysql tpires/sonar-mysql
3e8a23c2216d770d1ec43da34ed6b3b57db52788267c09dde83e5b7e306399a5
```

Die Option `-p` sorgt für eine Port-Weiterleitung von dem Port 3306 im Container
auf den Port 3306 von `localhost`. Mit `--name` bekommt der Container einen
Namen zugewiesen. Diesen Namen nutze ich bei der Erstellung des zweiten
Containers zur Erzeugung der Verknüpfung (des Links):

```shell
sudo docker.io run -i -t -d --name sonar -p 9000:9000 --link smysql:db tpires/sonar-server

8550f0ed7794c169d91e88157f5fe37a670f37ed5171313d6bcbf8787733082f
```

Das war es schon! Damit ist SonarQube im Web-Browser über die Adresse `http://localhost:9000` erreichbar.  

Für einen schnellen Test der Installation habe ich die Datei `build.gradle` eines HelloWorld-Projekts wie folgt erweitert:

<pre class="lang:default decode:true " title="Integration des Plugins sonarRunner" >apply plugin: 'sonar-runner'

sonarRunner {
    sonarProperties {
        property "sonar.host.url", "http://localhost:9000"
        property "sonar.jdbc.url", "jdbc:mysql://localhost:3306/sonar"
        property "sonar.jdbc.driverClassName", "com.mysql.jdbc.Driver"
        property "sonar.jdbc.username", "sonar"
        property "sonar.jdbc.password", "123qwe"
    }
}</pre>

Nach einem `gradle sonarRunner` erscheint auch prompt die Beurteilung des HelloWorld-Projekts im Web-Browser.
