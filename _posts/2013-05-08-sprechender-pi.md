---
title: Sprechender Pi
date: 2013-05-08T21:59:10+02:00
author: Stefan Freitag
tags: 
  - raspberry-pi
---
Im Netz bin ich auf einen interessanten [Post](http://web.archive.org/web/20151016182428/http://danfountain.com:80/2013/03/raspberry-pi-text-to-speech/ "Raspberry Pi Text To Speech") gestoßen. In diesem Post ist ein sehr einfacher Weg beschrieben, wie man den Pi zum Sprechen bringt. Ich hatte vor wenigen Monaten selber eine kleine Java-Applikation geschrieben, die das Text To Speech-Feature des Google Translators nutzt, aber für mein kommendes &#8222;Projekt&#8220; ist das im Post zu findende Bash-Skript einfach praktischer.

Für das Abspielen der vom Google Translator zurückgelieferten Daten wird `mpg123` verwendet und ist zuvor zu installieren.

```shell
apt-get install mpg123
```

Das folgende Skript ist im Vergleich zum Original-Post um einige
Kommentar-Zeilen erleichtert und um eine Einrückung ergänzt - ich liebe es
halt übersichtlich!

```shell
#!/bin/bash
#################################
# Speech Script by Dan Fountain #
# TalkToDanF@gmail.com #
#################################
INPUT=$*
STRINGNUM=0
ary=($INPUT)
echo "---------------------------"
echo "Speech Script by Dan Fountain"
echo "TalkToDanF@gmail.com"
echo "---------------------------"
for key in "${!ary[@]}"
do
  SHORTTMP[$STRINGNUM]="${SHORTTMP[$STRINGNUM]} ${ary[$key]}"
  LENGTH=$(echo ${#SHORTTMP[$STRINGNUM]})

  if [[ "$LENGTH" -lt "100" ]]; then
    SHORT[$STRINGNUM]=${SHORTTMP[$STRINGNUM]}
  else
    STRINGNUM=$(($STRINGNUM+1))
    SHORTTMP[$STRINGNUM]="${ary[$key]}"
    SHORT[$STRINGNUM]="${ary[$key]}"
  fi
done
for key in "${!SHORT[@]}"
do
  echo "Playing line: $(($key+1)) of $(($STRINGNUM+1))"
  mpg123 -q "http://translate.google.com/translate_tts?tl=en&q=${SHORT[$key]}"
done
```

Zur Ausführung des Skripts werden noch die Berechtigungen angepasst:

```shell
sudo chmod +x 
```

Das Skript wird in Verbindung mit dem zu sprechenden Text als Argument aufgerufen, z.B.

```shell
./speech.sh  Scarlett O Hara was not beautiful, but men seldom realized it when caught by her
charm as the Tarleton twins were.
```

Durch den Austausch von `tl=en` durch `tl=de` wechselst man übrigens zur
deutschen Phonetik.
