---
title: "Übersicht collectors"
date: 2018-08-10T22:23:00+02:00
anchor: "uebersicht-collectors"
weight: 21
---

Mithilfe von _collectors_ können Werte verschiedener Literalen
aggregiert, gefiltert oder zusammengefasst werden. Fast alle von ihnen teilen sich zwei Eigenschaften:

- Der Zeitpunkt und die Art der Emission ihrer Werte ist
  [steuerbar]({{< ref "/collectors/ausgabesteuerung.md" >}}).
- Es ist möglich, sie nur dann auszuführen, wenn [gewisse Bedingungen]({{< ref "/collectors/if-anweisungen.md" >}}) erfüllt
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

Folgend eine Liste der verfügbare _collectors_ in Metamorph.

{{% notice note %}}
Die Attribute `flushWith`, `reset` und `sameEntity`, welche für die
meisten _collectors_ verwendet werden können, werden im [nächsten
Abschnitt]({{< ref "ausgabesteuerung.md" >}})
beschrieben.
{{% /notice %}}

## combine
{{< icon name="fa-github" >}}[`combine`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Combine.java) fügt Literale gemäss einem Template zusammen. Nachfolgende Werte
überschreiben vorangehenden Werte desselben Feldes.

- `name` (erforderlich): Name des neuen Feldes
- `value` (erforderlich): Template. Werte von Feldern können mit
  `${feldname}` eingefügt werden

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ farbeA: grün, farbeA: gelb, farbeB: orange, farbeB: violet }
```

__Morph-Definition__

```xml
<combine name="mischung" value="${a}${b}">
  <data source="farbeA" name="a"/>
  <data source="farbeB" name="b"/>
</combine>
```

__Ausgabe__

```
{ mischung: gelborange, mischung: gelbviolet }
```

{{% /panel %}}
{{% /expand %}}

## concat
{{< icon name="fa-github" >}}[`concat`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Concat.java) fügt Literale in der angegebenen Reihenfolge in einem neuen Feld zusammen.
Werte desselben Feldes werden konkateniert.

- `name` (erforderlich): Name des neuen Feldes
- `delimiter`: Trennzeichen zwischen einzelnen Literalen (Standard: `""`)
- `prefix`: Präfix des neuen Feldes
- `postfix`: Suffix des neuen Feldes
- `reverse`: Fügt neue Literale am Anfang statt am Ende des neuen Feldes
  hinzu

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ farbeA: grün, farbeA: gelb, farbeB: orange, farbeB: violet }
```

__Morph-Definition__

```xml
<concat name="mischung" delimiter="">
  <data source="farbeA" name="a"/>
  <data source="farbeB" name="b"/>
</concat>
```

__Ausgabe__

```
{ mischung: grüngelborangeviolet }
```

{{% /panel %}}
{{% /expand %}}

## entity
{{< icon name="fa-github" >}}[`entity`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Entity.java) erstellt mit den definierten Literalen eine neue (Sub-)Entität.

`name` definiert den Namen der Entität, der alternativ auch mit dem
`entity-name`-Element definiert werden kann. Wird weder `name` noch
`entity-name` angegeben, wird ein leerer Name gesendet.

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ farbeA: rot, farbeB: grün, farbeC: blau, farbeD: magenta }
```

__Morph-Definition__

```xml
<entity name="rgb" flushWith="record">
  <data source="farbeA"/>
  <data source="farbeB"/>
  <data source="farbeC"/>
</entity>
<data source="_else" />
```

{{% notice note %}}
`flushWith="record"` sorgt dafür, dass die Literale erst am Ende des
Datensatzes weitergeleitet werden. Für Details siehe das [nächste
Kapitel]({{< ref "ausgabesteuerung.md" >}}).
{{% /notice %}}

__Ausgabe__

```
{ farbeD: magenta, rgb { farbeA: rot, farbeB: grün, farbeC: blau }}
```

{{% /panel %}}
{{% /expand %}}

## square
{{< icon name="fa-github" >}}[`square`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Square.java) fügt Literale zu allen möglichen nicht-permutierten Zweierkombinationen
zusammen. Werte desselben Feldes werden nicht überschrieben.

- `name` (erforderlich): Name der neuen 2er-Tupel
- `delimiter` (erforderlich): Trennzeichen zwischen den ausgegebenen Literalen
- `prefix`: Präfix der 2er-Tupel
- `postfix`: Suffix der 2er-Tupel

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ litA: wertA1, litA: wertA2, litB: wertB1, litC: wertC1 }
```

__Morph-Definition__

```xml
<square name="paar" delimiter="-" prefix="@" postfix="@">
  <data source="litA"/>
  <data source="litB"/>
  <data source="litC"/>
</square>
```

__Ausgabe__

```
{ paar: @wertA1-wertC1@, paar: @wertA2-wertC1@, paar: @wertB1-wertC1@, paar: @wertA1-wertB1@, paar: @wertA2-wertB1@, paar: @wertA1-wertA2@ }
```

{{% /panel %}}
{{% /expand %}}

## tuples
{{< icon name="fa-github" >}}[`tuples`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Tuples.java) gibt alle möglichen nicht-permutierten Kombinationen der Literalwerte aus.

- `name` (erforderlich): Name der neuen Literale
- `separator`: Trennzeichen zwischen den Literalen

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ litA: wertA1, litA: wertA2, litB: wertB }
```

__Morph-Definition__

```xml
<tuples name="gruppen" separator="-">
  <data source="litA"/>
  <data source="litB"/>
</tuples>
```

__Ausgabe__

```
{ gruppen: wertA1-wertB, gruppen: wertA2-wertB }
```

{{% /panel %}}
{{% /expand %}}

## group
{{< icon name="fa-github" >}}[`group`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Group.java) gruppiert Literale und/oder _collectors_, normalerweise um ein gemeinsames Set an Funktionen auf sie anzuwenden.

- `name`: Neuer gemeinsamer Name
- `value`: Neuer gemeinsamer Wert

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ kleinA: abc, kleinB: def }
```

__Morph-Definition__

```xml
<group name="GROSS">
  <data source="kleinA"/>
  <data source="kleinB"/>
  <postprocess>
    <case to="upper"/>
  </postprocess>
</group>
```

__Ausgabe__

```
{ GROSS: ABC, GROSS: DEF }
```

{{% /panel %}}
{{% /expand %}}

## choose
{{< icon name="fa-github" >}}[`choose`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Choose.java) sendet nur einen Literal aus einer Gruppe weiter (mit abnehmender
Priorität).

- `name`: Name des neuen Literals
- `value`: Wert des neuen Literals

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ wichtig: wichtig, vernachlässigbar: vernachlässigbar, unwichtig: unwichtig }
```

__Morph-Definition__

```xml
<data source="litA">
  <compose prefix="be" postfix="ung"/>
</data>
```

__Ausgabe__

```
{ wichtig: wichtig }
```

{{% /panel %}}
{{% /expand %}}

## range
{{< icon name="fa-github" >}}[`range`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Range.java) wertet zwei aufeinanderfolgende Literale als ganze Zahlen aus, die den Start- und
Endpunkt einer Zahlenreihe definieren. Alle Zahlen dieser Reihe werden
anschliessend als Literale ausgegeben.

- `name` (obligatorisch): Name der neuen Literale
- `increment`: Inkrement (Standard: `1`)

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ anfang: 0, ende: 10 }
```

__Morph-Definition__

```xml
<range name="gerade zahl" increment="2">
  <data source="anfang"/>
  <data source="ende"/>
</range>
```

__Ausgabe__

```
{ gerade zahl: 0, gerade zahl: 2, gerade zahl: 4, gerade zahl: 6, gerade zahl: 8, gerade zahl: 10 }
```

{{% /panel %}}
{{% /expand %}}

## equalsFilter
{{< icon name="fa-github" >}}[`equalsFilter`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/EqualsFilter.java) gibt ein neues Literal aus, sofern die Werte der Literale alle gleich sind.

- `name` (obligatorisch): Name des neuen Literals
- `value` (obligatorisch): Wert des neuen Literals

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{ litA: gleich, litA: gleich, litB: gleich }
```

__Morph-Definition__

```xml
<equalsFilter name="alle waren gleich" value="ja!">
  <data source="litA"/>
  <data source="litB"/>
</equalsFilter>
```

__Ausgabe__

```
{ alle waren gleich: ja! }
```

{{% /panel %}}
{{% /expand %}}

## all, any, none
{{< icon name="fa-github" >}}[`all`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/All.java), {{< icon name="fa-github" >}}[`any`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Any.java) und {{< icon name="fa-github" >}}[`none`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/None.java) überprüfen, ob angegebenen Literale vorhanden sind. Sie geben standardmässig eine
unbenannte (änderbar mit `name=`) Variable mit `true` als Wert (änderbar mit
`value=`) aus, insofern die jeweilige Bedingung zutrifft:

- Bei `all`: Alle Literale sind im Datensatz vorhanden
- Bei `any`: Mindestens ein Literal ist im Datensatz vorhanden
- Bei `none`: Kein Literal ist im Datensatz vorhanden

{{% notice note %}}
Die Verwendung der drei Quantoren macht insbesondere Sinn im Zusammenhang mit
[`if`-Anweisungen]({{< ref "/collectors/if-anweisungen.md" >}}).
{{% /notice %}}

{{% expand "Beispiel" %}}
{{% panel %}}
__Eingabe__

```
{litB: wertB}
```

__Morph-Definition__

```xml
<all name="alle vorhanden">
  <data source="litA"/>
  <data source="litB"/>
</all>
<any name="einige vorhanden">
  <data source="litA"/>
  <data source="litB"/>
</any>
<none name="keine vorhanden">
  <data source="litA"/>
  <data source="litB"/>
</none>
```

__Ausgabe__

```
{einige vorhanden: true}
```

{{% /panel %}}
{{% /expand %}}

