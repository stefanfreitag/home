---
title: Installation eines VOMRS Servers auf Scientific Linux 5
date: 2012-08-07T21:29:29+02:00
author: Stefan Freitag
tags: 
  - vomrs
  - sl5
  - grid
---

Hallo zusammen 🙂

Da auch nach dem möglichen Auffinden des Higgs-Teilchens das Grid in Deutschland
in Form des [NGI-DE](http://www.ngi-de.eu/) weiter brummen wird, hier nochmal
die Schritte um einen VOMRS Server aufzusetzen.

Zur Erinnerung: Der VOMRS dient den virtuellen Organisationen des Grid als -
vereinfacht gesprochen - Mitgliederverzeichnis. Neben den so genannten VOMRS
Administratoren, denen Verwaltungsaufgaben obliegen, gibt es beispielsweise
VOMRS Repräsentanten, deren Hauptaufgabe es ist, Mitgliedsanträge zeitnah zu
bearbeiten. Aber zuvor muss der Dienst natürlich installiert werden.

- Auf dem VOMRS Server ist eine Zeitsynchronisation, etwa mittels NTP Daemon,
  sinnvoll. Der Daemon kann nach seiner Konfiguration zukünftig wie folgt
  automatisch gestartet werden
  
  ```shell
  chkconfig --levels 2345 ntpd on
  /etc/init.d/ntpd start</pre>
  ```

- Die Weiterleitung der an `root` gerichteten e-Mails einrichten. Dazu als
  Nutzer `root` im home-Verzeichnis eine Datei `.forward` anlegen und die
  Zieladresse der Weiterleitung eintragen.
  
  ```shell
  cd ~
  touch .forward
  ```

- Nicht sehr schön, aber wir machen es trotzdem: Ausschalten der `enforcing` policy von SELinux in der Datei `/etc/selinux/config`

  ```shell
  sed -i 's/SELINUX=enforcing/SELINUX=permissive/g' /etc/selinux/config
  ```

- Einen Neustart des Systems durchführen, damit die Änderungen an SELinux gültig werden. (Alternativ ein `echo 0 > /selinux/enforce` ausführen)

  ```shell
  shutdown -r now
  ```

Soviel zu den Basics. Wie viele andere Dienste im Grid, setzt auch der VOMRS auf X.509-basierte Zertifikate. Diese werden nachfolgend eingespielt.

- Installation des Host-Zertifikats und des Host-Schlüssels in das Verzeichnis `/etc/grid-security/`
  
  ```shell
  mkdir -p /etc/grid-security
  cp some_path/hostcert.pem /etc/grid-security/hostcert.pem
  chmod 644 /etc/grid-security/hostcert.pem
  cp some_path/hostkey.pem /etc/grid-security/hostkey.pem
  chmod 400 /etc/grid-security/hostkey.pem
  ```

- Installation eines HTTP-Servicezertifikats und -schlüssels in das Verzeichnis `/etc/grid-security/http/`. Vereinfachend wird das Host-Zertifikat als Servicezertifikat verwendet.

  ```shell
  mkdir -p /etc/grid-security/http
  cp /etc/grid-security/hostcert.pem /etc/grid-security/http/httpcert.pem
  chown daemon:daemon /etc/grid-security/http/httpcert.pem 
  cp /etc/grid-security/hostkey.pem /etc/grid-security/http/httpkey.pem
  chown daemon:daemon /etc/grid-security/http/httpkey.pem
  ```

Als nächstes erfolgt die eigentliche Installation des VOMRS mittels Pacman.

- Herunterladen von Pacman
  
  ```shell
  cd ~
  wget http://atlas.bu.edu/~youssef/pacman/sample_cache/tarballs/pacman-latest.tar.gz
  ```

- Extrahieren des heruntergeladenen Archivs
  
  ```shell
  tar xzf pacman-latest.tar.gz
  ```

- Pacman vorbereiten, z.B. durch Festlegen und Anlegen des VOMRS Installationsverzeichnisses

  ```shell
  cd pacman-3.29/
  source setup.sh
  export VDT_LOCATION=/opt/vdt
  mkdir -p $VDT_LOCATION
  cd $VDT_LOCATION
  ```

- Den Paket-Cache von Pacman aktualisieren, die dabei auftretende Frage mit `yall`beantworten.
  
  ```shell
  pacman -get http://vdt.cs.wisc.edu/vdt_200_cache/:VOMRS
  Do you want to add [http://vdt.cs.wisc.edu/vdt_200_cache/] to [trusted.caches]? (y/n/yall):
  ```

- Nach dem Durchlauf der Installation auszuführende Aktionen

  ```shell
  source setup.sh
  vdt-control --enable vdt-rotate-logs
  vdt-control --on vdt-rotate-logs  # Rotieren der log-Dateien

  vdt-ca-manage setupca --location root --url vdt
  vdt-control --enable vdt-update-certs
  vdt-control --on vdt-update-certs # Aktualisieren der CA Zertifikate
  vdt-control --enable fetch-crl    # Abholen der CRLs
  ```

Nach dem Abschluss der Installation kann man die Liste der installierten Dienste anzeigen per

```shell
vdt-control -list
```

beziehungsweise Dienste einschalten per

```shell
vdt-control --on 
vdt-control --enable vomrs
```

Falls zwischenzeitlich über einen fehlenden SMTP Server gemeckert wurde, kann
dessen Einrichtung nachgeholt werden. Im Beispiel wird angenommen, dass dieser
auf `localhost` als Mail Relay läuft. Mit dem zusätzlichen Schalter
`--mail-from` (hier nicht gezeigt) lässt sich noch der Absender der e-Mails
festlegen.

```shell
vdt/setup/configure_vomrs --smtp-host localhost
```

## Einrichtung der SMTP-Konfiguration für `localhost`

Der zuvor beschrieben VOMRS Server nutzt zum Versenden von e-Mail ein Mail Relay, welches auf `localhost` betrieben wird. Dieser Abschnitt beschreibt die Konfiguration dieses Relays

- Installation des Pakets `sendmail-cf`

  ```shell
  yum install -y sendmail-cf
  ```

- Einzelne Änderungen an der Datei `/etc/mail/sendmail.mc`vornehmen
  - Einkommentieren der `SMART_HOST`-Definition und des `FEATURE`s. Der Wert für `SMART_HOST`entspricht dem später tatsächlich genutzten SMTP-Server, also dem Server an den das Mail Relay alle Mails weiterleitet.
  
    ```shell
    define(`SMART_HOST', `SMTP_SERVER')
    FEATURE(authinfo)dnl
    ```

  - Anlegen einer neuen Datei `sendmail.cf`

    ```shell
    /etc/init.d/sendmail stop
    m4 /etc/mail/sendmail.mc &gt; /etc/mail/sendmail.cf
    ```

- Informationen zur Authentifizierung am SMTP Server bereitstellen.
  - Erzeugen der Datei `/etc/mail/authinfo` mit folgendem Inhalt:

    ```shell
     AuthInfo:<your-smtp-server> "U:<your-smtp-user>" "P:<your-smtp-password>" "M:PLAIN"
    ```

    PLAIN ist nicht wirklich ideal und sollte später gegen DIGEST-MD5 oder CRAM-MD5 ausgetauscht werden.)

  - Erzeugen der Datei `authinfo.db`

    ```shell
    makemap hash /etc/mail/authinfo.db &lt; /etc/mail/authinfo
    ```

  - Neustart von sendmail

    ```shell
    /etc/init.d/sendmail restart
    ```

Und fertig ist&#8217;s 🙂