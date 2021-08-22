---
title: 'Task Warrior &#8211; Intermezzo'
date: 2014-08-04T21:08:54+02:00
author: Stefan Freitag
---

Meine letzten Aktivitäten mit [Dashing](http://dashing.io/ "Dashing Homepage")
haben mich nochmal zu meinem Lieblingswerkzeug [Task
Warrior](http://taskwarrior.org/ "Task Warrior Homepage") geführt. Task Warrior
läuft @work auf meinem [Raspberry Pi](http://de.wikipedia.org/wiki/Raspberry_Pi
"Wikipedia zu Raspberry Pi") und dient mir als gute Gedächtnisstütze bei der
Projektarbeit.  
Aktuell verwalte ich die einzelnen Tasks und Projekte über die Kommandozeile,
aber es gibt auch andere Möglichkeiten - s z.B. über eine Web-Schnittstelle wie
[taskwarrior-web](https://github.com/theunraveler/taskwarrior-web "Git-Seite zu
TaskWarrior Web ").

Die Installation der Software geht zügig vonstatten und es wird dazu lediglich
ein Befehl benötigt:

```shell
$ sudo gem install taskwarrior-web
Fetching: parseconfig-1.0.4.gem (100%)
Fetching: vegas-0.1.11.gem (100%)
Fetching: rinku-1.7.3.gem (100%)
Building native extensions.  This could take a while...
Fetching: blockenspiel-0.4.5.gem (100%)
Building native extensions.  This could take a while...
Fetching: versionomy-0.4.4.gem (100%)
Fetching: i18n-0.6.11.gem (100%)
Fetching: activesupport-3.2.19.gem (100%)
Fetching: simple-navigation-3.13.0.gem (100%)
Fetching: sinatra-simple-navigation-3.7.0.gem (100%)
Fetching: rack-flash3-1.0.5.gem (100%)
Fetching: json-1.7.7.gem (100%)
Building native extensions.  This could take a while...
Fetching: taskwarrior-web-1.1.11.gem (100%)
Successfully installed parseconfig-1.0.4
Successfully installed vegas-0.1.11
Successfully installed rinku-1.7.3
Successfully installed blockenspiel-0.4.5
Successfully installed versionomy-0.4.4
Successfully installed i18n-0.6.11
Successfully installed activesupport-3.2.19
Successfully installed simple-navigation-3.13.0
Successfully installed sinatra-simple-navigation-3.7.0
Successfully installed rack-flash3-1.0.5
Successfully installed json-1.7.7
Successfully installed taskwarrior-web-1.1.11
12 gems installed
Installing ri documentation for parseconfig-1.0.4...
Installing ri documentation for vegas-0.1.11...
Installing ri documentation for rinku-1.7.3...
Installing ri documentation for blockenspiel-0.4.5...
Installing ri documentation for versionomy-0.4.4...
Installing ri documentation for i18n-0.6.11...
Installing ri documentation for activesupport-3.2.19...                                                                                                                                         
Installing ri documentation for simple-navigation-3.13.0...               
Installing ri documentation for sinatra-simple-navigation-3.7.0...         Installing ri documentation for rack-flash3-1.0.5...                       Installing ri documentation for json-1.7.7...                              Installing ri documentation for taskwarrior-web-1.1.11...                  Installing RDoc documentation for parseconfig-1.0.4...                     Installing RDoc documentation for vegas-0.1.11...                          Installing RDoc documentation for rinku-1.7.3...                           Installing RDoc documentation for blockenspiel-0.4.5...                    Installing RDoc documentation for versionomy-0.4.4...                      Installing RDoc documentation for i18n-0.6.11...                           Installing RDoc documentation for activesupport-3.2.19...                  Installing RDoc documentation for simple-navigation-3.13.0...              Installing RDoc documentation for sinatra-simple-navigation-3.7.0...       Installing RDoc documentation for rack-flash3-1.0.5...                     Installing RDoc documentation for json-1.7.7...                            Installing RDoc documentation for taskwarrior-web-1.1.11...
```

Nach der Installation kann die Web-Schnittstelle mit folgendem Aufruf gezündet
werden:

```shell
$ task-web 
[2014-08-04 20:55:24 +0200] Starting 'taskwarrior-web'...
[2014-08-04 20:55:24 +0200] trying port 5678...
Konnte keinen Dateideskriptor finden, der auf die Konsole verweist.
```

Als Teil der Ausgabe erhält man auch die Information auf welchen Port (hier:
1)    man sich verbinden muss.
