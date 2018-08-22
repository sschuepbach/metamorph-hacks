---
title: "Filter"
date: 2018-08-10T21:31+02:00
anchor: "filter"
weight: 13
---

## [`whitelist`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/WhiteList.java)
Filtert Werte basierend auf einer Whitelist (nur gelistete Werte werden
weitergeleitet)

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

## [`blacklist`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/BlackList.java)
Filtert Werte basierend auf einer Blacklist (gelistete Werte werden
geblockt)

{{% notice info %}}
Für weitere Informationen siehe Abschnitt
["Nachschlagetabellen"]({{< ref "nachschlagetabellen.md" >}})
{{% /notice %}}

## [`equals`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Equals.java) / [`not-equals`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/NotEquals.java)
Literal wird nur dann weitergeleitet, wenn sein Wert gleich wie Wert in `string=` ist.
Für `not-equals` gilt das Umgekehrte.

## [`occurrence`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Occurrence.java)
Literal werden nur dann weitergeleitet, wenn sie mehr (`moreThan`) oder weniger
oft (`lessThan`) als ein ein Wert (`only=`) vorkommen. Bsp.:

```xml
<data source="fieldA">
  <occurrence only="moreThan 4" />
</data>
```

## [`unique`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Unique.java)
Filtert doppelte Werte

- `in`: Gleiche Werte in Entität (`entity`) oder Datensatz (`record`,
  Standard)
- `part`: Teile des Literals, die gleich sein müssen: `name`, `value`
  (Standard) oder `name-value`

{{% notice note %}}
Ein Wert in `part` abweichend von `value` ist nur dann sinnvoll, wenn
_globs_ für den Feldnamen verwendet werden.
{{% /notice %}}

## [`buffer`](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/java/org/metafacture/metamorph/functions/Buffer.java)
Hält Literale zurück, bis _flush_-Bedingung (`flushWith=`) erfüllt wird.

{{% notice info %}}
Für weitere Informationen zu `flushWith` siehe Abschnitt ["Ausgabesteuerung"]({{< ref "/collectors/ausgabesteuerung.md" >}}).
{{% /notice %}}
