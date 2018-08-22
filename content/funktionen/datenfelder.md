---
title: "Werte auslesen"
date: 2018-08-10T20:51:00+02:00
anchor: "datenfelder"
weight: 11
---

Werte einzelner Datenfelder können mit der Funktion `data` ausgelesen werden.
In ihrer einfachsten Form hat `data` ein Attribut, `source`, welches dem Namen
des auszulesenden Feldes entspricht. Dabei können auch Wildcards verwendet
werden, so `?` für ein beliebiges Zeichen und `*` für Sequenz arbiträrer
Zeichen mit Länge 0-n. In dieser einfachsten Form - ausgeschrieben also bspw.
`<data source="100*.a"/>` - wird lediglich definiert, dass Werte in den
Feldern, welche auf den Ausdruck `100*.a` passen, ebenso wie die Feldnamen
unverändert übernommen werden sollen.

Zudem gibt es für `source` zwei Variablen mit fixer Bedeutung:

- `_id`: Liest die ID des Datensatzes aus.
- `_else`: Prozessiert alle Literale, welche nicht durch eine andere
  Morph-Regel verarbeitet werden. Nützlich, wenn Literale nicht
transformiert, aber auch nicht ausgefiltert werden sollen.

`data` nimmt aber noch ein zweites Attribut entgegen, `name`. Dadurch lassen
sich Feldnamen umbenennen, während der Feldwert noch immer unverändert
übernommen wird. Durch den Feldnamen `_id` wird die ID des Datensatzes
überschrieben. 

{{% notice note %}}
Datenfelder mit Namen, die mit `@` beginnen, werden [speziell]({{< ref "/modularisierung/rekursion.md" >}}) behandelt.
{{% /notice %}}

Schliesslich lassen sich auf Feldwerte eine Reihe von Transformationen und
Filter anwenden.
