---
title: Rekursion
date: 2018-08-13T17:29:00+02:00
anchor: rekursion
weight: 31
---

Normalerweise wird ein Literal direkt nach seiner Verarbeitung an das nächste
_command_ weitergeleitet. Es ist durch eine spezielle Syntax - ein führendes
`@` im Namen - allerdings möglich, Werte bis zum Prozessierungsende des
Datensatzes im Cache zu halten:

```xml
<data source="eingabename" name="@loop"/>
<data source="@loop" name="ausgabename"/>
```

Dadurch lassen sich verschiedene Probleme lösen, so:

- Eine initiale Verarbeitung eines Literals lässt sich zentral definieren, die
  Ausgabe lässt sich an verschiedenen Orten im Morph wiederverwenden
(beispielsweise in einem _collector_)
- Das Ergebnis eines _collector_, der vor Ende des Datensatzes "geflusht" werden muss, soll in einem anderen _collector_ genutzt werden

{{% notice warning %}}
Literale im Cache müssen bei jeder Wiederverwendung __umbenannt__ werden, da
sonst eine unendliche Schleife entsteht (aka ein `StackOverflowError`).
{{% /notice %}}

{{% notice note %}}
Ein literales `@` als erstes Zeichen eines Feldnamens ist möglich, wenn es
durch einen vorangehenden `\` seiner speziellen Bedeutung "beraubt"
wird (das `\` erscheint anschliessend nicht mehr im Namen). Das `\` ist
nicht notwendig, wenn sich das betreffende Literal bzw. der betreffende
_collector_ innerhalb eines Eltern-_collectors_ befindet (siehe dazu auch
nächste Box).
{{% /notice %}}

{{% notice warning %}}
Literale bzw. _collectors_, welche innerhalb eines _collectors_ in die Rekursion geschickt werden, werden gleichzeitig immer auch literal weitergeleitet (also bspw. als `("@feldname": "feldwert")`). Da dieses Verhalten ist in der Regel nicht gewünscht, deshalb sollten Rekursionen grundsätzlich ausserhalb von Eltern-_collectors_ definiert werden. 
{{% /notice %}}

{{% notice note %}}
Rekursion mit der `@`-Syntax kann das Problem nicht vollständig beheben, dass
Literale in der Reihenfolge abgearbeitet werden, in der sie in das
_morph-command_ geschickt werden. So ist es beispielsweise meines Wissens nicht
möglich, alle Werte mit demselben Feldnamen in einem _collector_
wiederzuverwenden.
{{% /notice %}}
