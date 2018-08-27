---
title: "Transformationen"
date: 2018-08-10T21:27+02:00
anchor: "transformationen"
weight: 12
---

Dieses Kapitel gibt eine Übersicht über Literalfunktionen, welche
Literalwerte ändern oder ersetzen. Wie die im nächsten Kapitel besprochenen
Filterfunktionen werden sie innerhalb des `<data>`-Elementes eingefügt und
können, wo sinnvoll, beliebig miteinander kombiniert werden. Dabei wird der
Wert von oben nach unten von der einen zur nächsten Funktion
durchgereicht. Ein Beispiel:

```xml
<data source="litA" name="gruss">
  <trim/>
  <whitelist>
    <entry name="hallo"/>
    <entry name="grüezi"/>
  </whitelist>
  <case to="upper"/>
</data>
```

## compose
{{< icon name="fa-github" >}}[`compose`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Compose.java)
fügt dem Wert ein Prä- (`prefix=`) und/oder ein Suffix (`postfix=`) hinzu

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: zeug}
```

__Morph-Definition__

```xml
<data source="litA">
  <compose prefix="be" postfix="ung"/>
</data>
```

__Ausgabe__

```
{litA: bezeugung}
```

{{% /panel %}}
{{% /expand %}}

## constant
{{< icon name="fa-github"
>}}[`constant`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Constant.java)
ersetzt Wert durch ein Literal (`value=`)

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: sinnlos}
```

__Morph-Definition__

```xml
<data source="litA">
  <constant value="sinnvoll"/>
</data>
```

__Ausgabe__

```
{litA: sinnvoll}
```

{{% /panel %}}
{{% /expand %}}

## timestamp
{{< icon name="fa-github" >}}[`timestamp`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Timestamp.java) ersetzt Wert durch aktuellen Zeitstempel

- `format`: Format des Zeitstempels gemäss
  [`java.text.SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
- `timezone`: Zeitzone gemäss
  [`java.util.TimeZone`](https://docs.oracle.com/javase/8/docs/api/java/util/TimeZone.html)
(Standard: `UTC`)
- `language`: Gebietsschema gemäss
  [`java.util.Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{}
```

__Morph-Definition__

```xml
<data source="_id" name="zeitstempel">
  <timestamp format="yyyy-MM-dd HH:mm:ss" timezone="GMT+02:00" language="de"/>
</data>
```

__Ausgabe__

```
{zeitstempel: 2018-08-24 17\:59\:18}
```

{{% /panel %}}
{{% /expand %}}

## substring
{{< icon name="fa-github" >}}[`substring`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Substring.java) extrahiert Substring basierend auf Indizes (`start=`, `end=`)

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: bezeugung}
```

__Morph-Definition__

```xml
<data source="litA">
  <substring start="2" end="6"/>
</data>
```

__Ausgabe__

```
{litA: zeug}
```

{{% /panel %}}
{{% /expand %}}

## regexp
{{< icon name="fa-github" >}}[`regexp`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Regexp.java) extrahiert einen Substring basierend auf einem regulären Ausdruck (`match=`).
Rückwärtsreferenzen (_capture groups_) werden unterstützt, welche durch
`format="${1..n}"` wiederverwendet werden können. Syntax entspricht derjenigen
von [regulären Ausdrücken in Java](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html).

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: eins zwei drei}
```

__Morph-Definition__

```xml
<data source="litA">
  <regexp match="\S+ (\S+) \S+" format="${1} bücher"/>
</data>
```

__Ausgabe__

```
{litA: zwei bücher}
```

{{% /panel %}}
{{% /expand %}}

## replace / setreplace
{{< icon name="fa-github" >}}[`replace`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Replace.java) / {{< icon name="fa-github" >}}[`setreplace`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/SetReplace.java) ersetzt einen passenden Substring (`pattern=`) durch einen anderen Wert
(`with=`). Java-konforme reguläre Ausdrücke werden unterstützt. Durch
`setreplace` lässt sich eine ganze lokale Ersetzungstabelle definieren
(`<entry name="ausdruck" value="ersetzung"/>`) oder mithilfe von `map=` eine
solche referenzieren.

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: eins, litA: zwei, litA: drei, litA: vier}
```

__Morph-Definition__

```xml
<data source="litA" name="zahl">
  <setreplace>
    <entry name="eins" value="1"/>
    <entry name="zwei" value="2"/>
    <entry name="drei" value="3"/>
    <entry name="vier" value="4"/>
  </setreplace>
</data>
```

__Ausgabe__

```
{litA: 1, litA: 2, litA: 3, litA: 4}
```

{{% /panel %}}
{{% /expand %}}

## lookup
{{< icon name="fa-github" >}}[`lookup`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Lookup.java) ist analog zu [`setreplace`](#replace-setreplace), unterstützt aber nur statische Strings als
Suchmuster. Mithilfe von `in=` kann alternativ auch eine Konkordanztabelle
referenziert werden. `default=` definiert einen Standardwert, wenn Schlüssel
nicht in Tabelle definiert ist.

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{kanton: Waadt, kanton: Wallis, kanton: Jura, kanton: Genf}
```

__Morph-Definition__

```xml
<data source="kanton" name="hauptort">
  <lookup default="nicht bekannt">
    <entry name="Wallis" value="Sion"/>
    <entry name="Jura" value="Delémont"/>
    <entry name="Waadt" value="Lausanne"/>
  </lookup>
</data>
```

__Ausgabe__

``` 
{hauptort: Lausanne, hauptort: Sion, hauptort: Delémont, hauptort: nicht bekannt}
```

{{% /panel %}}
{{% /expand %}}

## case
{{< icon name="fa-github" >}}[`case`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Case.java) wandelt Wert in Gross- (`upper`) bzw. Kleinbuchstaben (`lower`) (`to=`) um. Nimmt optional ein
[Sprach-Tag](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html) entgegen (`language=`)

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{buch: kitabı}
```

__Morph-Definition__

```xml
<data source="buch">
  <case to="upper" language="tr"/>
</data>
```

__Ausgabe__

``` 
{buch: KİTABI}
```

{{% /panel %}}
{{% /expand %}}

## trim
{{< icon name="fa-github" >}}[`trim`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Trim.java) schneidet Leerzeichen am Ende des Feldwertes ab

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: 'luftig   '}
```

__Morph-Definition__

```xml
<data source="litA">
  <trim />
</data>
```

__Ausgabe__

``` 
{litA: luftig}
```

{{% /panel %}}
{{% /expand %}}

## normalize-utf8
{{< icon name="fa-github" >}}[`normalize-utf8`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/NormalizeUTF8.java) normalisiert Zeichen in kanonische Form, bspw. ein `ä`, das aus dem Diakrit `¨` + `a` gebildet wurde, zu `ä`. Für Details siehe [hier](http://www.unicode.org/reports/tr15/tr15-23.html).

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: ä }
```

__Morph-Definition__

```xml
<data source="litA">
  <normalize-utf8 />
</data>
```

__Ausgabe__

``` 
{litA: ä}
```

{{% /panel %}}
{{% /expand %}}

## dateformat
{{< icon name="fa-github" >}}[`dateformat`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/DateFormat.java) formatiert Datum

- `inputformat`: Format des Inputdatums gemäss
  [`java.text.SimpleDateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
- `outputformat`: Format des Ausgabedatums: `FULL`, `LONG` (Standard), `MEDIUM` oder
  `SHORT` (s. Beschreibung in
[`java.text.DateFormat`](https://docs.oracle.com/javase/8/docs/api/java/text/DateFormat.html))
- `era`: Epoche: `AUTO` (Standard), `AD` oder `BC` 
- `removeLeadingZeros`: Entferne führende Nulle (Standard: `false`)
- `language`: Gebietsschema gemäss
  [`java.util.Locale`](https://docs.oracle.com/javase/8/docs/api/java/util/Locale.html)

{{% notice warning%}}
`removeLeadingZeros="true"` entfernte in unseren Tests fälschlicherweise auch Nulle in
vierstelligen Jahreszahlen, das Resultat war also bspw. `218` anstelle von `2018`!
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{heute: 2018-08-26 }
```

__Morph-Definition__

```xml
<data source="heute">
  <dateformat inputformat="yyyy-MM-dd" outputformat="FULL" era="AD" removeLeadingZeros="false" language="de"/>
</data>
```

__Ausgabe__

``` 
{heute:Sonntag\, 26. August 2018}
```

{{% /panel %}}
{{% /expand %}}

## split
{{< icon name="fa-github" >}}[`split`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Split.java) teilt Feldwert basierend auf regulärem Ausdruck (`delimiter=`) in Substrings

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{heute: 2018-08-26 }
```

__Morph-Definition__

```xml
<data source="heute" name="monat">
  <split delimiter="-"/>
  <occurrence only="moreThan 1"/>
  <occurrence only="lessThan 2"/>
</data>
```

{{% notice note %}}
[`occurrence`]({{< ref "filter.md#occurrence" >}}) ist ein Filter, welche nur
Werte weiterleitet, die bis zu oder ab einem bestimmten Index in einer Liste
von Werten vorkommen.
{{% /notice %}}

__Ausgabe__

``` 
{monat: 10}
```

{{% /panel %}}
{{% /expand %}}

## isbn
{{< icon name="fa-github" >}}[`isbn`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/ISBN.java) transformiert ISBN-Nummern. Das Attribut `to` akzeptiert drei Werte:
`isbn10` für eine Umwandlung von ISBN13 zu ISBN10, `isbn13` für das Umgekehrte
und `clean` für eine Bereinigung der Nummer. Optional können zudem die
Attribute `verifyCheckDigit`, standardmässig `false`, und `errorString`
gesetzt werden.

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{isbn: 978-3-89401-810}
```

__Morph-Definition__

```xml
<data source="isbn">
  <isbn to="isbn10" verifyCheckDigit="true" errorString="ungültig"/>
</data>
```

__Ausgabe__

``` 
{isbn:ungültig}
```

{{% /panel %}}
{{% /expand %}}

## urlencode
{{< icon name="fa-github" >}}[`urlencode`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/URLEncode.java) enkodiert Wert als URL

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{suchwert: gelebtes leben}
```

__Morph-Definition__

```xml
<data source="suchwert">
  <urlencode />
  <compose prefix="https://swissbib.ch/Search/Results?lookfor="/>
</data>
```

__Ausgabe__

``` 
{suchwert:https\://swissbib.ch/Search/Results?lookfor=gelebtes+leben}
```

{{% /panel %}}
{{% /expand %}}

## htmlanchor
{{< icon name="fa-github" >}}[`htmlanchor`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/HtmlAnchor.java) baut aus Wert einen HTML-Link. Umfasst drei Attribute: `prefix` (zwingend),
`postfix` und `title` (optional)

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{artikel: mein interessanter beitrag}
```

__Morph-Definition__

```xml
<data source="anker">
  <urlencode />
  <htmlanchor prefix="https://example.com/" title="Mein interessanter Beitrag"/>
</data>
```

__Ausgabe__

``` 
{anker:<a href="https\://example.com/mein+interessanter+beitrag">Mein interessanter Beitrag</a>}
```

{{% /panel %}}
{{% /expand %}}

## switch-name-value
{{< icon name="fa-github" >}}[`switch-name-value`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/SwitchNameValue.java) vertauscht Feldname und -wert

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: wertA}
```

__Morph-Definition__

```xml
<data source="litA">
  <switch-name-value/>
</data>
```

__Ausgabe__

``` 
{wertA: litA}
```

{{% /panel %}}
{{% /expand %}}

## script
{{< icon name="fa-github" >}}[`script`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Script.java) übergibt Wert zur Transformation an eine Javascript-Funktion (`invoke=`) in einer
externen Datei (`file=`)

{{% notice note %}}
Hinweise zur Implementierung einer solchen Funktion finden sich im [Anhang]({{< ref "/anhang/metamorph-erweitern.md#erweitern-mit-einer-javascript-funktion" >}})
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{palindrom: Regallager}
```

__Morph-Definition__

```xml
<data source="litA">
  <script file="stringutils.js" invoke="reverse"/>
</data>
```

{{% notice note %}}
`stringutils.js`, das eine Funktion `reverse` enthält, muss sich in diesem Fall im gleichen Verzeichnis befinden.
{{% /notice %}}

__Ausgabe__

``` 
{palindrom: regallageR}
```

{{% /panel %}}
{{% /expand %}}

## java
`java` übergibt Wert zur Tranformation einer Java-Klasse (`class=`,
voller Name). Werte
aller weiteren Attribute werden einer gleichnamigen _setter_-Methode der
Klasse übergeben.

{{% notice note %}}
Hinweise zur Implementierung einer solchen Klasse finden sich im [Anhang]({{< ref "/anhang/metamorph-erweitern.md#erweitern-mit-einer-java-klasse" >}})
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{dekodiert: Eine strenggeheime Botschaft!}
```

__Morph-Definition__

```xml
<data source="dekodiert" name="enkodiert">
  <java class="com.example.secret.Encoder" algorithm="ROT13" />
</data>
```

{{% notice note %}}
Damit das Beispiel funktioniert, müsste ein Metafacture-Plugin die Klasse
`Encoder` im angegebenen Klassenpfad zur Verfügung stellen. Zudem muss
die Klasse eine _setter_-Methode `setAlgorithm` haben.
{{% /notice %}}

__Ausgabe__

``` 
{enkodiert: Rvar fgerattrurvzr Obgfpunsg!}
```

{{% /panel %}}
{{% /expand %}}

## count
{{< icon name="fa-github" >}}[`count`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Count.java) gibt Anzahl der aktuell gezählten Werte aus.

{{% notice note %}}
Um nur den letzten Wert zu berücksichtigen, siehe das Rezept ["Nur letzten Wert
eines Literals berücksichtigen"]({{< ref "/kochbuch/nur-letzten-wert-beruecksichtigen.md" >}}).
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: wert1, litA: wert2}
```

__Morph-Definition__

```xml
<data source="litA">
  <count/>
</data>
```

__Ausgabe__

```
{litA: 1, litA: 2}
```

{{% /panel %}}
{{% /expand %}}
