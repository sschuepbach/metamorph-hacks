---
title: Rekursion
date: 2018-08-13T17:29:00+02:00
anchor: rekursion
weight: 31
---

Normalerweise wird ein Literal direkt nach seiner Verarbeitung an das nächste
_command_ weitergeleitet. Es ist durch eine spezielle Syntax - ein führendes
`@` im Namen - allerdings möglich, seine Werte bis zum Prozessierungsende des
Datensatzes im Cache zu halten:

```xml
<data source="eingabename" name="@loop"/>
<data source="@loop" name="ausgabename"/>
```

Bei der Verwendung dieser Syntax sind einige Dinge zu beachten:

- Literale im Cache müssen bei jeder Wiederverwendung __umbenannt__ werden, da
sonst eine unendliche Schleife entsteht (aka ein `StackOverflowError`).
Das heisst also, dass beispielsweise folgendes Konstrukt __nicht__ möglich ist:
    ```xml
    <data source="eingabename" name="@loop"/>
    <data source="@loop"/> <!-- name-Attribut fehlt! -->
    ```

- Ein literales `@` als erstes Zeichen eines Feldnamens ist möglich, wenn es
durch einen vorangehenden `\` seiner speziellen Bedeutung "beraubt"
wird (`\` erscheint anschliessend nicht mehr im Namen). `\` ist
nicht notwendig, wenn sich das betreffende Literal bzw. der betreffende
_collector_ innerhalb eines Eltern-_collectors_ befindet (siehe dazu auch
den nächsten Punkt).
- Literale bzw. _collectors_, welche innerhalb eines anderen _collectors_ in die Rekursion geschickt werden, werden gleichzeitig immer auch literal weitergeleitet (also bspw. als `("@feldname": "feldwert")`). Da dieses Verhalten ist in der Regel nicht gewünscht, deshalb sollten Rekursionen grundsätzlich ausserhalb von Eltern-_collectors_ definiert werden. 

Durch Rekursion lassen sich verschiedene Probleme lösen, so:

- Eine initiale Verarbeitung eines Literals lässt sich zentral definieren, die
  Ausgabe lässt sich an verschiedenen Orten im Morph wiederverwenden
(beispielsweise in einem _collector_)
- Das Ergebnis eines _collector_, der vor Ende des Datensatzes "geflusht" werden muss, soll in einem anderen _collector_ genutzt werden

Allerdings gibt es auch Einschränkungen. Der gecachte Literal verhält sich aus Sicht der ihn aufrufen Morph-Definition wie ein Iterator: Jeder seiner Werte kann nur einmal wiederverwendet werden und geht dann verloren. Es ist zudem nicht möglich, einen gecachten Literal gestaffelt abzuarbeiten.

{{% panel header="Beispiel" %}}

__Eingabe__

```
{ litA: wertAA, litA: wertAB, litB: wertBA, litB: wertBB }
```

__Morph-Definitionen__

```xml
<rules>
    <data source="litA" name="@loop"/>
    <entity name="entAB" flushWith="litB" reset="true">
        <data source="@loop" name="litA"/>
        <data source="litB" name="litB"/>
    </entity>
</rules>
```

__Ausgabe__

``` 
{ entAB { litA: wertAA, litA: wertAB, litB: wertBA }, entAB { litB: wertBB } }
```

{{% /panel %}}
