---
title: Metamorph erweitern
date: 2018-08-24T22:37:00+02:00
anchor: metamorph-erweitern
weight: 91
---

Metamorph kann mit eigenen Java-Klassen oder Javascript-Funktionen
erweitert werden. Während im Kapitel "Transformationen" bereits die beiden
entsprechenden Morph-Funktionen, [`java`]({{< ref "/literale/transformationen.md#java" >}}) und [`script`]({{< ref "/literale/transformationen.md#script" >}}), vorgestellt wurden,
werden an dieser Stelle Hinweise zur Implementation gegeben. Da dies weniger
mit Metamorph selbst als mit der API von Metafacture zu tun hat,
befindet sich dieses Kapitel im Anhang.

## Erweitern mit einer Java-Klasse

Das Metafacture-Framework stellt im Paket `org.metafacture.metamorph-api`
verschiedene abstrakte Klassen zur Verfügung, um Metamorph mit eigenen
Java-Klassen zu erweitern:

- [`AbstractSimpleStatelessFunction`](https://github.com/metafacture/metafacture-core/blob/master/metamorph-api/src/main/java/org/metafacture/metamorph/api/helpers/AbstractSimpleStatelessFunction.java) für Funktionen, die keinen Zustand zwischen der Verarbeitung einzelner Werte speichern müssen
- [`AbstractFilter`](https://github.com/metafacture/metafacture-core/blob/master/metamorph-api/src/main/java/org/metafacture/metamorph/api/helpers/AbstractFilter.java), eine Kindklasse von `AbstractSimpleStatelessFunction`, welche für Filterfunktionen genutzt werden kann
- [`AbstractStatefulFunction`](https://github.com/metafacture/metafacture-core/blob/master/metamorph-api/src/main/java/org/metafacture/metamorph/api/helpers/AbstractStatefulFunction.java) für Funktionen, die Werte von Literalen aggregieren

Die Prozessierung des Literalwertes erfolgt jeweils durch die Methode
`protected String process(String value)`, die den Literalwert entgegennimmt
und den modifizierten zurückgibt. Durch _setter_-Methoden kann Instanzen
dieser Klasse eine beliebige Anzahl an Argumenten mitgegeben werden (vgl. dazu
die Metamorph-Funktion [`java`]({{< ref "/literale/transformationen.md#java"
>}})).

Folgend ein Beispiel für eine neue zustandslose Metamorph-Funktion, welche
Strings mit dem [ROT13](https://de.wikipedia.org/wiki/ROT13)-Algorithmus
enkodiert:

```java
package org.swissbib.linked;

import org.metafacture.metamorph.api.helpers.AbstractSimpleStatelessFunction;

public class Encoder extends AbstractSimpleStatelessFunction {

    private String algorithm = "ROT13";

    public void setAlgorithm(String name) {
        this.algorithm = name;
    }

    @Override
    protected String process(String value) {
        StringBuilder result = new StringBuilder();
        switch (algorithm) {
            case "ROT13":
                for (char c : value.toCharArray()) {
                    if (c >= 65 && c <= 90) {
                        result.append(rotate(c, (char) 65, (char) 90));
                    } else if (c >= 97 && c <= 122) {
                        result.append(rotate(c, (char) 97, (char) 122));
                    } else {
                        result.append(c);
                    }
                }
        }
        return result.toString();
    }

    private char rotate(char c, char min, char max) {
        if ((c + 13) > max) {
            return (char) (c + 13 - max + min - 1);
        } else {
            return (char) (c + 13);
        }
    }
}
```

## Erweitern mit einer Javascript-Funktion

Die Javascript-Funktion muss ein Argument - den Wert des Literals -
entgegennehmen können und ein Wert eines beliebigen Typs zurückgeben. Die die
Funktion aufrufende
[Java-Klasse](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Script.java)
wendet dann die `toString()`-Methode auf den Rückgabewert an und gibt den Wert
an die verarbeitende Metamorph-Funktion zurück. Ein Beispiel:

```javascript
// script.js
// Funktioniert nur für Strings mit ASCII-Zeichen!
function asciiReverse(val){
  return val.split("").reverse().join("");
}
```

In Metamorph wird die Funktion folgendermassen aufgerufen:
```xml
<data source="litA">
  <!-- Pfade sind relative zur Basis-URI -->
  <script file="script.js" envoke="asciiReverse"/>
</data>
```
