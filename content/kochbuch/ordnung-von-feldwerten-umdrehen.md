---
title: Ordnung von Werten in einem Literal umdrehen
date: 2018-08-24T10:46:00+02:00
anchor: ordnung-von-feldwerten-umdrehen
weight: 42
---

## Problem

Ein Literal (`litA`) hat mehrere Werte in der Reihenfolge `wert1`,
`wert2`. Diese Reihenfolge soll umgedreht werden.

__Eingabe__

```
{ litA: wert1, litA: wert2 }
```

__Erwartete Ausgabe__

```
{ litA: wert2, litA: wert1 }
```

## Lösung

```xml
<concat name="litA" delimiter="-" reverse="true">
  <data source="litA"/>
  <postprocess>
    <split delimiter="-"/>
  </postprocess>
</concat>
```

Werte werden mit `concat` in einem String, wobei neue Werte mit
`reverse="true"` jeweils vorne angefügt werden. Anschliessend wird mit der
Funktion `split` der konkatenierte Wert entlang eines Zeichens (hier `-`)
wieder aufgetrennt. Natürlich sollte das verwendete Zeichen nicht als Wert im
Literal vorkommen, deshalb ist in der Praxis die Verwendung eines selteneren Zeichens als `-` ratsam.
