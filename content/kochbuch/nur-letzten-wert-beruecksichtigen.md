---
title: Nur letzten Wert eines Literals berücksichtigen
date: 2018-08-24T12:13:00+02:00
anchor: nur-letzten-wert-beruecksichtigen
weight: 44
---

## Problem

Von einem Literal (`litA`) sollen die Werte gezählt werden, doch
interessiert nur die letzte Zählung (das heisst die Zählung über alle Werte).

__Eingabe__

```
{ litA: wert1, litA: wert2 }
```

__Erwartete Ausgabe__

```
{ litA: 2 }
```

## Lösung

```xml
<combine name="litA" value="${litA}" flushWith="record">
  <data source="litA">
    <count />
  </data>
</combine>
```

Die Lösung macht sich zunutze, dass `combine` Werte überschreibt. Wird erst am
Ende des Datensatzes weitergeleitet, verbleibt also nur die Zählung über
alle Werte von `litA`.
