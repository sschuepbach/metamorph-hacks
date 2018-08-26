---
title: "Filter"
date: 2018-08-10T21:31+02:00
anchor: "filter"
weight: 13
---

Filterfunktionen filtern Literalwerte regelbasiert aus. In der Folge ein
Überblick über die diesbezüglichen Möglichkeiten in Metamorph.

## whitelist
{{< icon name="fa-github" >}}[`whitelist`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/WhiteList.java) filtert Werte basierend auf einer Whitelist (nur gelistete Werte werden
weitergeleitet). Einträge sind in der Form `<entry
name="wert"/>`.

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: erlaubt, litA: nicht erlaubt, litA: verboten, litA: indiziert, litA: genehm}
```

__Morph-Definition__

```xml
<data source="litA">
  <whitelist>
    <entry name="erlaubt"/>
    <entry name="genehm"/>
  </whitelist>
</data>
```

__Ausgabe__

```
{litA:erlaubt, litA:genehm}
```

{{% /panel %}}
{{% /expand %}}

## blacklist
{{< icon name="fa-github" >}}[`blacklist`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/BlackList.java) filtert Werte basierend auf einer Blacklist (gelistete Werte werden
geblockt). Einträge sind in der Form `<entry name="wert"/>`.


{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: erlaubt, litA: nicht erlaubt, litA: verboten, litA: indiziert, litA: genehm}
```

__Morph-Definition__

```xml
<data source="litA">
  <blacklist>
    <entry name="nicht erlaubt"/>
    <entry name="verboten"/>
    <entry name="indiziert"/>
  </blacklist>
</data>
```

__Ausgabe__

```
{litA:erlaubt, litA:genehm}
```

{{% /panel %}}
{{% /expand %}}

## equals / not-equals
{{< icon name="fa-github" >}}[`equals`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Equals.java) / {{< icon name="fa-github" >}}[`not-equals`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/NotEquals.java) leiten Literal
nur dann (nicht) weiter, wenn sein Wert gleich wie Wert in `string=` ist.

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: passt, litA: passt nicht}
```

__Morph-Definition__

```xml
<data source="litA">
  <equals string="passt"/>
</data>
<!-- Gleichbedeutend -->
<data source="litA">
  <not-equals string="passt-nicht"/>
</data>
```

__Ausgabe__

```
{litA: passt}
```

{{% /panel %}}
{{% /expand %}}

## occurrence
{{< icon name="fa-github" >}}[`occurrence`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Occurrence.java) leitet Literal ab (`moreThan`) oder bis
(`lessThan`) zu einer gewissen Anzahl an Werten (`only=`) weiter.

{{% notice note %}}
In Kombination können `moreThan` und `lessThan`-Filter eingesetzt werden, um einen Teil aus einer Liste von Werten zu extrahieren (siehe Beispiel unten)
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: wert1, litA: wert2, litA: wert3, litA: wert4}
```

__Morph-Definition__

```xml
<data source="litA" name="dritterWert">
  <occurrence only="moreThan 2"/>
  <occurrence only="lessThan 2"/>
</data>
```

__Ausgabe__

```
{dritterWert: wert3}
```

{{% /panel %}}
{{% /expand %}}

## unique
{{< icon name="fa-github" >}}[`unique`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Unique.java) filtert doppelt vorkommende Werte aus

- `in`: Gleiche Werte in Entität (`entity`) oder Datensatz (`record`,
  Standard)
- `part`: Teile des Literals, die gleich sein müssen: `name`, `value`
  (Standard) oder `name-value`

{{% notice note %}}
Ein Wert in `part` abweichend von `value` ist nur dann sinnvoll, wenn
_globs_ für den Feldnamen verwendet werden.
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: wertA, litA: wertB, litA: wertA, litB: wertA }
```

__Morph-Definition__

```xml
<data source="lit?">
  <unique part="name-value" />
</data>
```

__Ausgabe__

```
{litA: bezeugung}
```

{{% /panel %}}
{{% /expand %}}

## buffer
{{< icon name="fa-github" >}}[`buffer`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Buffer.java) hält Literale zurück, bis _flush_-Bedingung (`flushWith=`) erfüllt wird.

{{% notice info %}}
Für weitere Informationen zu `flushWith` siehe Abschnitt ["Ausgabesteuerung"]({{< ref "/collectors/ausgabesteuerung.md" >}}).
{{% /notice %}}

{{% notice warning %}}
Wird `flushWith=` mit einem bestimmten Feldnamen angegeben, muss das
entsprechende Literal als explizite Morph-Regel aufgeführt werden, und nicht
etwa nur durch ein `<data source="_else"/>`! Siehe dazu auch das Beispiel unten. Dies scheint ein Bug zu sein.
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litA: wertA, litB: wertB, litC: wertC}
```

__Morph-Definition__

```xml
<data source="litA">
  <buffer flushWith="litC"/>
</data>
<!-- litC muss explizit aufgeführt werden, da es ansonsten gar nicht ausgegeben wird. Siehe auch Hinweis oben. -->
<data source="litC"/>
<data source="_else"/>
```

__Ausgabe__

```
{litB: wertB, lit }
```

{{% /panel %}}
{{% /expand %}}
