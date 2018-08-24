---
title: Ordnung von Literalen ändern
date: 2018-08-24T10:55:00+02:00
anchor: ordnung-von-literalen-aendern
weight: 43
---

## Problem

Ein Literal (`litA`) wird vor einem anderen (`litB`) prozessiert. Nun soll aber diese Reihenfolge umgedreht werden. 

__Eingabe__

```
{ litA: wertA, litB: wertB }
```

__Erwartete Ausgabe__

```
{ litB: wertB, litA: wertA }
```

## Lösung

```xml
<data source="litA">
  <buffer flushWith="record"/>
</data>
<data source="_else"/>
```

Mit der Funktion `buffer` lässt sich ein Literal an einer beliebigen Stelle
weiterleiten, in diesem Fall erst am Ende des Datensatzes anstatt gleich
nach der Bearbeitung des entsprecechenden Literals. Mit `<data
source="_else"/>` wird sichergestellt, dass auch Literale
weitergeleitet werden, für die keine explizite Regel besteht. 


## Bemerkungen

Sollen mehrere Literale in ihrer Reihenfolge verändert werden, ist es
sinnvoll, mehrere `morph`-_commands_ hintereinander zu schalten, und in
jeder Morph-Datei ein Literal an das Ende zu verschieben (`<data
source="_else"/>` nicht vergessen!).
