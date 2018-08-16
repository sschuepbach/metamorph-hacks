---
title: "Collectors"
date: 2018-08-10T22:15+02:00
anchor: "collectors"
weight: 20
---

Mithilfe von _collectors_ können Werte verschiedener Literalen
aggregiert, gefiltert oder zusammengefasst werden. Fast alle von ihnen teilen sich zwei Eigenschaften:

- Der Zeitpunkt und die Art der Emission ihrer Werte ist
  [steuerbar](#ausgabesteuerung).
- Es ist möglich, sie nur dann auszuführen, wenn [gewisse Bedingungen](#if-anweisungen) erfüllt
  sind. 

Zudem ist es für die _collectors_ `combine`, `concat`, `equalsFilter`,
`square`, `tuples`, `choose`, `range` und `group` möglich, mittels
`<postprocess>` Literal-Funktionen auf das Ergebnis anzuwenden. Beispiel:

```xml
<combine name="neu" value="${nachname}, ${vorname}">
  <data source="feld1" name="vorname"/>
  <data source="feld2" name="nachname"/>
  <postprocess>
    <trim/>
    <case to="upper"/>
  </postprocess>
</combine>
```
