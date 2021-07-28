---
title: Software-Development im Netz date: 2015-03-21T13:54:03+01:00 layout: post
---
Je länger ich aktiv Software entwickle - sowohl beruflich also auch privat -
desto mehr wird klar, dass ein gesundes Umfeld notwendig ist. Es reicht einfach
nicht aus, Quellcode in eine
[IDE](http://de.wikipedia.org/wiki/Integrierte_Entwicklungsumgebung) einzugeben
und zu meinen, dass damit alles getan ist.

Die ersten beiden Schritte, die dem Schreiben von Quellcode folgen, durchlaufen
die meisten Entwickler in unterschiedlicher Reihenfolge, kommen aber
letztendlich am selben Punkt an. Sie nutzen

- Unit Tests (und ggf. Integration Tests), um ihre Implementierung zu prüfen.
- eine Versionsverwaltung, um ihren inkrementellen Fortschritt festzuhalten und
  vielleicht auch auf frühere Revisionen zurückzublicken oder diese
  wiederherzustellen.

Wer mehr als eine IDE zu bedienen hat, bei mir waren es zwischenzeitlich mit
[Netbeans](https://netbeans.org/ "Netbeans Homepage"),
[Eclipse](https://eclipse.org/ "Eclipse Homepage") und
[IntelliJ](https://www.jetbrains.com/idea/ "IntelliJ IDEA Homepage") drei, der
wünscht sich zudem ein IDE-unabhängiges Verfahren ([Ant](http://ant.apache.org/
"Ant Homepage"), [Maven](http://maven.apache.org/ "Maven Homepage") und
[Gradle](https://gradle.org/ "Gradle Homepage") seien hier als Repräsentanten
genannt) zum Bauen der Software.

Mit der Zeit gestaltet man sich ein kleines Universum, um den eigentlichen
Prozess der Software-Entwicklung zu unterstützen. Im privaten Bereich ist mein
Universum zuletzt ein gutes Stück expandiert. Ein wichtiges Kriterium dabei war
die nur sehr geringe Lust selber Services & Server zu betreiben ("No additional
ballast, please"). Des Weiteren sollte die Nutzung der externen Services
kostenlos möglich sein.

Wie also sieht mein Universum nun eigentlich aus?

- IntelliJ IDEA  
  Kurz gesagt: es läuft und läuft und läuft. Dabei ist die IDE nicht so
  Ressourcen-hungrig wie Eclipse und in manchen Belangen einfach intuitiver.
  Aber die IDE ist ja nur einer kleiner Teil dieses Kosmos.
- Gradle  
  Wie schon angedeutet, brauchte ich einen Weg, um beim Bauen der Software
  eine Unabhängigkeit von der verwendeten IDE zu erreichen. Gradle war mir bis
  jetzt ein guter Begleiter und ließ sich problemlos in alle oben genannten
  IDEs integrieren.
- Git  
  Als ich an der [TU
  Dortmund}(http://www.tu-dortmund.de/uni/Uni/index.html) meine Diplomarbeit
  geschrieben habe, war [CVS](http://cvs.nongnu.org/) weit verbreitet.
  Allerdings etablierte sich bereits langsam
  [Subversion](https://subversion.apache.org/) zu dem neuen Standard. Mit dem
  Wunsch, auf ein zentrales Repository zu verzichten, bin ich nun bei
  [Git](http://git-scm.com/) angelangt.
- [GitHub](https://github.com/ "GitHub Homepage")  
  Da ich nicht im Elfenbeinturm vor mich hinleben mag, möchte ich meinen Code
  (Bibliotheken, Applikationen, etc.) mit anderen Interessierten teilen.
  Bislang hatte ich dazu den Quellcode,  die Dokumentation und das Endprodukt
  als Download bereitgestellt.  GitHub scheint mir zumindest für die ersten
  beiden Punkte eine gute Lösung zu sein. Jeder kann sich dort meinen
  Quellcode anschauen, klonen & weiterentwickeln, aber auch einen Pull Request
  (zum Beispiel Wünsche nach Ergänzungen oder auch Fehlerkorrekturen)
  einreichen.
- [StillMaintained](http://stillmaintained.com/ "Still Maintained Homepage")  
  Ist der Quellcode in Form von Projekten auf GitHub online, stellt sich die
  Frage wie lange man diese jeweils aktiv weitertreibt und betreut. Um
  Außenstehenden auf einfachem Wege eine Information über diesen Punkt
  zukommen zu lassen, habe ich mich zur Nutzung von StillMaintained
  entschieden. Im Grunde versieht man dabei sein Projekt mit einem der
  folgenden Stati: maintained, searching, abandoned.
- [Travis CI](https://travis-ci.org)  
  Die eingangs erwähnten Unit Test laufen sicherlich bestens auf meinem
  Entwicklungsrechner. Für ein aussagekräftiges Ergebnis müssen die Tests
  allerdings auf einem anderen Rechner bzw. System ausgeführt werden. Travis CI
  koppelt sich an meine auf GitHub veröffentlichten Projekte, baut nach neuen
  Commits eigenständig das Projekt und lässt mir bei Problemen eine e-Mail
  zukommen.
- [BinTray](http://bintray.com/)  
  BinTray bildet quasi das Ende meiner Kette. Der Quellcode ist online, die
  Tests laufen durch, alles super also. Aber wie kommt man nun an die
  Applikation an sich? Keiner der vorherigen Dienste deckt diesen Punkt ab. Auf
  BinTray kann ich für einzelne Releases meiner Software Binärpakete
  bereitstellen und diese über unterschiedliche Mechanismen zugreifbar machen:
  zum Beispiel als Maven-Repository oder auch als Repository für deb- oder
  rpm-Pakete.
