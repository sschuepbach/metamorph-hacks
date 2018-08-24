---
title: Literale in Entitäten wiederverwenden
date: 2018-08-21T16:22:00+02:00
anchor: literale-in-entitaeten-wiederverwenden
weight: 41
---

## Problem
Ein Literal (`litA`) soll mit einem mehrfach vorhandenen weiteren Literal (`litB`) in einer Entität zusammenfügt werden, wobei für jedes `litB` eine Entität erstellt wird.

__Eingabe__

```
{litA: wertA, litB: wertB, litB: wertC}
```

__Erwartete Ausabe__

```
{entAB {litA: wertA, litB: wertB} entAB {litA: wertA, litB: wertC}}
```

## Lösung

```xml
<entity name="entAB" flushWith="litB" reset="true">
    <combine name="litA" value="${litA}" flushWith="litB">
        <data source="litA"/>
    </combine>
    <data source="litB" name="litB"/>
</entity>
```

`litA` wird in einem `combine`-_collector_ "gespeichert", der in diesem
Fall wie ein Cache benutzt werden kann. Wichtig ist, dass der
_collector_ zur gleichen Zeit weitergeleitet wird wie die ihn umgebende
`entity`, da ansonsten der in ihm aggregierte Wert nur einmal (nämlich wenn er
das erste Mal gefüllt ist) "geflusht" wird.

{{% notice note %}}
Analoges gilt für `concat`.
{{% /notice %}}

## Bemerkungen

Andere, vermutlich naheliegendere Strategien funktionieren nicht

### Direktes Auslesen des Literals

```xml
<entity name="entAB" flushWith="litB" reset="true">
    <data source="litA"/>
    <data source="litB" name="litB"/>
</entity>
```

Ausgabe:

```
{entAB {litA: wertA, litB: wertB} entAB {litB: wertC}}
```

Da das Feld `litA` vor `litB` ausgelesen und im `entity`-Block  nach der Verarbeitung des ersten `litB` geflusht wird, steht es zum Zeitpunkt des zweiten `litB` nicht mehr zur Verfügung.

### Entität nicht zurücksetzen

```xml
<entity name="entAB" flushWith="litB" reset="false">
    <combine name="litA" value="${litA}" flushWith="litB">
        <data source="litA"/>
    </combine>
    <data source="litB" name="litB"/>
</entity>
```

Ausgabe:

```
{entAB {litA: wertA, litB: wertB} entAB {litA: wertA, litB: wertB, litB: wertC}}
```

Nicht möglich, da so __alle__ Werte in `entity` gesammelt werden. Allerdings
würde diese Strategie beispielsweise für `combine` funktionieren, da in diesem
Fall Werte mit gleichem Feldnamen überschrieben werden.  

### Rekursives Zugreifen auf litA

```xml
<data source="litA" name="@loop"/>
<entity name="entAB" flushWith="litB" reset="false">
    <data source="@loop" name="litA"/>
    <data source="litB" name="litB"/>
</entity>
```

Ausgabe:

```
{entAB {litA: wertA, litB: wertB} entAB {litB: wertC}}
```

Zwar ist der Wert von `litA` über die ganze restliche Prozessierungszeit des Records im Cache gespeichert, doch kann pro Zugriffsort nur einmal über ihn iteriert werden.
