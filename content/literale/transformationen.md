---
title: "Transformationen"
date: 2018-08-10T21:27+02:00
anchor: "transformationen"
weight: 12
---

## [`compose`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Compose.java)
Fügt dem Wert ein Prä- (`prefix=`) und/oder ein Suffix (`postfix=`) hinzu

## [`constant`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Constant.java)
Ersetzt Wert durch ein Literal (`value=`)

## [`timestamp`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Timestamp.java)
Ersetzt Wert durch aktuellen Zeitstempel

- `format`: Format des Zeitstempels gemäss
  [`java.text.SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
- `timezone`: Zeitzone gemäss
  [`java.util.TimeZone`](https://docs.oracle.com/javase/8/docs/api/java/util/TimeZone.html)
(Standard: `UTC`)
- `language`: Gebietsschema gemäss
  [`java.util.Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)

## [`substring`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Substring.java)
Extrahiert Substring basierend auf Indizes (`start=`, `end=`)

## [`regexp`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Regexp.java)
Extrahiert einen Substring basierend auf einem regulären Ausdruck (`match=`).
Rückwärtsreferenzen (_capture groups_) werden unterstützt, welche durch
`format="${1..n}"` wiederverwendet werden können. Syntax entspricht derjenigen
von regulären Ausdrücken in Java

## [`replace`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Replace.java) / [`setreplace`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/SetReplace.java)
Ersetzt einen passenden Substring (`pattern=`) durch einen anderen Wert
(`with=`). Reguläre Ausdrücke analog zu Java werden unterstützt. Durch
`setreplace` lässt sich eine ganze lokale Ersetzungstabelle definieren
(`<entry name="ausdruck" value="ersetzung"/>`) oder mithilfe von `map=` eine
solche referenzieren.

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

## [`lookup`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Lookup.java)
Analog zu `setreplace`, unterstützt aber nur statische Strings als
Suchmuster. Mithilfe von `in=` kann alternativ auch eine Konkordanztabelle
referenziert werden. `default=` definiert einen Standardwert, wenn Schlüssel
nicht in Tabelle definiert ist.

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

## [`case`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Case.java)
Zu Gross- (`upper`) bzw. Kleinschreibung (`lower`) (`to=`). Nimmt optional ein
Sprach-Tag entgegen (`language=`)

## [`trim`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Trim.java)
Schneidet Leerzeichen am Ende des Feldwertes ab

## [`normalize-utf8`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/NormalizeUTF8.java)
Normalisierung basierend auf UTF-8 (v.a. für Umlaute)

## [`dateformat`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/DateFormat.java)
Formatiert Datum

- `inputformat`: Format des Inputdatums gemäss
  [`java.text.SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
- `outputformat`: Format des Ausgabedatums: `FULL`, `LONG` (Standard), `MEDIUM` oder
  `SHORT` (s. Beschreibung in
[`java.text.DateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html))
- `era`: Epoche: `AUTO` (Standard), `AD` oder `BC` 
- `removeLeadingZeros`: Entferne führende Nulle (Standard: `false`)
- `language`: Gebietsschema gemäss
  [`java.util.Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)

## [`split`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Split.java)
Teilt Feldwert basierend auf regulärem Ausdruck (`delimiter=`) in Substrings

## [`isbn`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/ISBN.java)
Transformationen von ISBN-Nummern. Das Attribut `to` akzeptiert drei Werte:
`isbn10` für eine Umwandlung von ISBN13 zu ISBN10, `isbn13` für das Umgekehrte
und `clean` für eine Bereinigung der Nummer. Optional können zudem die
Attribute `verifyCheckDigit`, standardmässig `false`, und `errorString`
gesetzt werden.

## [`urlencode`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/URLEncode.java)
Enkodiert Wert als URL

## [`htmlanchor`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/HtmlAnchor.java)
Baut aus Wert einen HTML-Link. Umfasst drei Attribute: `prefix` (zwingend),
`postfix` und `title` (optional)

## [`switch-name-value`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/SwitchNameValue.java)
Vertauscht Feldname und -wert

## [`script`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Script.java)
Wert wird zur Transformation einer Javascript-Funktion (`invoke=`) in einer
externen Datei (`file=`) übergeben

## `java`
Wert wird zur Tranformation einer Java-Klasse (`class=`) übergeben. Werte
aller weiteren Attribute werden einer gleichnamigen _setter_-Methode in der
Klasse übergeben.

## [`count`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Count.java)
Ersetzt Wert durch Anzahl der spezifizierten Felder im Datensatz
