---

title: Plauschen mit dem SMTP-Server
date: 2012-11-25T13:45:33+01:00
author: Stefan Freitag
tags:
  - smtp
---
Mittlerweile ist man an den Einsatz von E-Mail Clients wie Thunderbird und
Outlook gewöhnt, doch man kann auch direkt mit einem SMTP-Server sprechen. Dazu
reicht das in allen Linux-Derivaten vorhandene `telnet`-Kommando aus.
Nachfolgend ist das ganze am Beispiel demonstriert, wobei wir hier den
einfachsten Fall annehmen, also keine großartige Form der verschlüsselten
Kommunikation.

- Zunächst muss man den SMTP-Server herausfinden, den man kontaktieren möchte.
  Dazu kann man beispielsweise `dig`verwenden.
  
  ```shell
  dig -t MX <domain_name>
  ```
  
  Für die Domain `zdf.de` gibt `dig`folgende Antwort:

  ```shell
  ;; ANSWER SECTION:

  zdf.de.    6554    IN      MX      15 inmail.zdf.
  ```

- Auf den SMTP-Server verbindet man sich per `telnet`auf Port 25. Ist der Server
  nett zu uns, so stellt er sich kurz vor, z.B.

  ```shell
  $ telnet <some_host> 25

  Trying AAA.BBB.CCC.DDD...
  Connected to .
  Escape character is '^]'.
  220  ESMTP Sendmail ...
  ```

- Danach stellt man sich selber dem SMTP-Server vor, in dem man HELO oder EHLO (Extened HELO) sagt.
  
  ```shell
  HELO
  ```
  
  Idealerweise wird das `HELO`mit einem Statuscode 250 erwidert.

  ```shell
  250  [...], pleased to meet you
  ```

- Im nächsten Schritt gibt man den Absender der e-Mail als Inhalt des `MAIL
  FROM`-Feldes an. Wichtig dabei ist die Verwendung der Spitzklammern.
  
  ```shell
  MAIL FROM:
  ```

- Die Festlegung des Empfängers stellt den nächsten Schritt dar. Das
  Eingabeformat entspricht dem im vorherigen Schritt verwendeten, lediglich das
  `MAIL FROM` weicht dem `RCPT TO`.

- Endlich kann der eigentliche Inhalt der e-Mail eingegeben werden. Dazu wird
  zunächst ein `DATA`an den SMTP-Server gesendet, danach sollte sich dieser mit
  dem Statuscode 354 und dem Hinweis

  ```shell
  Start mail input; end with .
  ```

  zurückmelden. Nun kann man die aus den e-Mails bekannten Informationen niederschreiben, z.B.

  ```shell
  From: Mir 
  To: Mir 
  Subject: Testmail


  Ich schreibe mir gerne selber e-Mails.
  ```

- Als letzten Schritt verabschieden wir uns vom Server durch ein `QUIT`.
