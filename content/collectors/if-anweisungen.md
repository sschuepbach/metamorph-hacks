---
title: "if-Anweisungen"
date: 2018-08-13T15:53:00+02:00
anchor: "if-anweisungen"
weight: 23
---

Mithilfe von `if`-Anweisungen lässt sich steuern, ob ein _collector_ Werte
ausgibt oder nicht. Die Syntax sieht folgendermassen aus:

```xml
<combine name="ausgabe" value="${a}+${b}">
  <if>
    <data source="feldA">
      <equals string="wertA"/>
    </data>
  </if>
  <data source="feldA" name="a"/>
  <data source="feldB" name="b"/>
</combine>
```

Wenn der Wert im `feldA` tatsächlich `wertA` ist, dann wird das Ergebnis
von `${a}+${b}` ausgegeben, ansonsten nicht.

In der if-Anweisung lässt sich jede [Filter-Funktion]({{< ref "/funktionen/filter.md" >}}) sowie die _collectors_ `all`,
`any` und `none` verwenden. Ein Beispiel mit `all`:


```xml
<combine name="ausgabe" value="${a}+${b}">
  <if>
    <all>
      <data source="feldA"/>
      <data source="feldB"/>
    </all>
  </if>
  <data source="feldA" name="a"/>
  <data source="feldB" name="b"/>
</combine>
```

Wenn beide Literale einen Wert liefern, wird das Ergebnis von `${a}+${b}`
weitergeleitet.
