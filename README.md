# Metamorph-Hacks

Dieses README beinhaltet Informationen zu Funktionen und _collectors_ in
Metamorph sowie eine Sammlung von Ansätzen ("Hacks"), wie komplexere
Transformationsworkflows in Metamorph bewerkstelligt werden können.

## Werte auslesen aus Datenfeldern

Werte einzelner Datenfelder können mit der Funktion `data` ausgelesen werden.
In ihrer einfachsten Form hat `data` ein Attribut, `source`, welches dem Namen
des auszulesenden Feldes entspricht. Dabei können auch Wildcards verwendet
werden, so `?` für ein beliebiges Zeichen und `*` für Sequenz arbiträrer
Zeichen mit Länge 0-n. In dieser einfachsten Form - ausgeschrieben also bspw.
`<data source="100*.a"/>` - wird lediglich definiert, dass Werte in den
Feldern, welche auf den Ausdruck `100*.a` passen, ebenso wie die Feldnamen
unverändert übernommen werden sollen.

`data` nimmt aber noch ein zweites Attribut entgegen, `name`. Dadurch lassen
sich Feldnamen umbenennen, während der Feldwert noch immer unverändert
übernommen wird.

Schliesslich lassen sich auf Feldwerte eine Reihe von Transformations-, und
Filterungsfunktionen anwenden.

## Transformationen

- `compose`: Fügt dem Wert ein Prä- (`prefix=`) und/oder ein Suffix
  (`postfix=`) hinzu
- `constant`: Ersetzt den Wert durch ein Literal (`value=`)
- `timestamp`:
- `substring`: Extrahiert Substring basierend auf Indizes (`start=`, `end=`)
- `regexp`: Extrahiert einen Substring basierend auf einem regulären Ausdruck
  (`match=`). Rückwärtsreferenzen (_capture groups_) werden unterstützt,
welche durch `format="${1..n}"` wiederverwendet werden können. Syntax
entspricht derjenigen von regulären Ausdrücken in Java
- `replace` / `setreplace`: Ersetzt einen passenden Substring (`pattern=`)
  durch einen anderen Wert (`with=`). Reguläre Ausdrücke analog zu Java werden
unterstützt. Durch `setreplace` lässt sich eine ganze lokale Ersetzungstabelle
definieren (`<entry name="ausdruck" value="ersetzung"/>`)
- `lookup`: Analog zu `setreplace`, unterstützt aber nur statische Strings als
  Suchmuster
- `case`: Zu Gross- (`upper`) bzw. Kleinschreibung (`lower`) (`to=`). Nimmt
  optional ein Sprach-Tag entgegen (`language=`)
- `trim`: Schneidet Leerzeichen am Ende des Feldwertes ab
- `normalize-utf8`: Normalisierung basierend auf UTF-8 (v.a. für Umlaute)
- `dateformat`: 
- `split`: Teilt Feldwert basierend auf regulärem Ausdruck (`delimiter=`) in
  Substrings
- `isbn`: Transformationen von ISBN-Nummern. Das Attribut `to` akzeptiert drei
  Werte: `isbn10` für eine Umwandlung von ISBN13 zu ISBN10, `isbn13` für das
Umgekehrte und `clean` für eine Bereinigung der Nummer. Optional können zudem
die Attribute `verifyCheckDigit`, standardmässig `false`, und `errorString`
gesetzt werden.
- `urlencode`: Enkodiert Wert als URL
- `htmlanchor`: Baut aus Wert einen HTML-Link. Umfasst drei Attribute:
  `prefix` (zwingend), `postfix` und `title` (optional)
- `switch-name-value`: Vertauscht Feldname und -wert
- `script`: Wert wird zur Transformation einer Javascript-Funktion (`invoke=`)
  in einer externen Datei (`file=`) übergeben
- `java`: Wert wird mit einem optionalen Präfix (`prefix=`) zur Tranformation
  einer Java-Klasse (`class=`) übergeben
- `count`: Ersetzt Wert durch Anzahl der spezifizierten Felder im Datensatz

## Filter

- `whitelist`:
- `blacklist`:
- `equals` / `not-equals`:
- `occurrence`:
- `unique`:
- `buffer`:



## Collectors und ihre Parameter

Metamorph umfasst sieben Typen von _collectors_, die alle mehrere Datenfelder
auslesen und deren Werte kombinieren können:

- `combine`: Kombiniert Werte verschiedener Felder
- `concat`: 
- `entity`:
- `choose`:
- `square`:
- `tuples`: 

## Der Gebrauch von if-statements
