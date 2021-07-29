---
title: Auflisten der vorhandenen Ciphers in Java
date: 2013-10-15T07:57:08+02:00
layout: post
author: Stefan Freitag
---
Eigentlich ist mit dem Titel des Blog-Eintrags schon alles gesagt. Wer sich die
Liste der über Java bzw. das [Java Cryptographic
Extension](http://de.wikipedia.org/wiki/Java_Cryptography_Extension) (JCE)
Framework unterstützten Ciphers anschauen mag, der kann dies über den folgenden
Code tun.  
(Die Ciphers entsprechen kryptographischen Algorithmen für die symmetrische/
asymmetrische Verschlüsselung von Daten).

```java
import java.security.Provider;
import java.security.Security;
import java.util.HashSet;
import java.util.Set;

public class AvailableCiphers {

    public static void main(String[] args) {
        String[] names = getAvailableCiphers("Cipher");
        for (String name : names){
            System.out.println(name);
        }
    }

    private static String[] getAvailableCiphers(final String serviceType) {
        Set<String> result = new HashSet<String>();

        Provider[] providers = Security.getProviders();

        for (Provider provider : providers) {
            Set<Object> keys = provider.keySet();

            for (Object keyObject : keys) {

                String key = (String) keyObject;
                key = key.split(" ")[0];

                if (key.startsWith(serviceType + ".")) {
                    result.add(key.substring(serviceType.length() + 1));
                } else if (key.startsWith("Alg.Alias." + serviceType + ".")) {
                    result.add(key.substring(serviceType.length() + 11));
                }
            }
        }
        return result.toArray(new String[result.size()]);
    }
}
```

Führe die den angegebenen Code auf meinem System aus, so erhalte ich folgende
Ausgabe:

```java
AvailableCiphers
OID.1.2.840.113549.1.12.1.6
Blowfish
DESedeWrap
Rijndael
DESede
PBEWithSHA1AndDESede
ARCFOUR
PBEWithSHA1AndRC2_40
RC2
RC4
RSA
AESWrap
PBEWithMD5AndTripleDES
OID.1.2.840.113549.1.12.1.3
DES
AES
1.2.840.113549.1.12.1.6
OID.1.2.840.113549.1.5.3
1.2.840.113549.1.12.1.3
1.2.840.113549.1.5.3
TripleDES
PBEWithMD5AndDES
```
