---
title: "Übersicht collectors"
date: 2018-08-10T22:23:00+02:00
anchor: "uebersicht-collectors"
weight: 21
---

Folgend eine Liste der verfügbare _collectors_ in Metamorph.

{{% block note %}}
Die Attribute `flushWith`, `reset` und `sameEntity`, welche für die
meisten _collectors_ verwendet werden können, werden im [nächsten
Abschnitt](#ausgabesteuerung)
beschrieben.
{{% /block %}}

## [`combine`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Combine.java)

Fügt Literale gemäss einem Template zusammen. Nachfolgende Werte
überschreiben vorangehenden Werte desselben Feldes.

- `name` (erforderlich): Name des neuen Feldes
- `value` (erforderlich): Template. Werte von Feldern können mit
  `${feldname}` eingefügt werden

## [`concat`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Concat.java)

Fügt Literale in der angegebenen Reihenfolge in einem neuen Feld zusammen.
Werte desselben Feldes werden konkateniert.

- `name` (erforderlich): Name des neuen Feldes
- `delimiter`: Trennzeichen zwischen einzelnen Literalen (Standard: `""`)
- `prefix`: Präfix des neuen Feldes
- `postfix`: Suffix des neuen Feldes
- `reverse`: Fügt neue Literale am Anfang statt am Ende des neuen Feldes
  hinzu

## [`entity`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Entity.java)

Erstellt mit den definierten Literalen eine neue (Sub-)Entität.
Nachfolgende Werte überschreiben vorangehende Werte desselben Feldes. 

`name` definiert den Namen der Entität, der alternativ auch mit dem
`entity-name`-Element definiert werden kann. Wird weder `name` noch
`entity-name` angegeben, wird ein leerer Name gesendet.

## [`square`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Square.java)

Fügt Literale zu allen möglichen nicht-permutierten Zweierkombinationen
zusammen. Werte desselben Feldes werden nicht überschrieben.

- `name` (erforderlich): Name der neuen 2er-Tupel
- `delimiter` (erforderlich): Trennzeichen zwischen den ausgegebenen Literalen
- `prefix`: Präfix der 2er-Tupel
- `postfix`: Suffix der 2er-Tupel

## [`tuples`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Tuples.java)

Gibt alle möglichen Kombinationen der definierten Literale aus.

- `name` (erforderlich): Name der neuen Literale
- `separator`: Trennzeichen zwischen den Literalen

## [`group`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Group.java)

Gruppiert Literale und/oder _collectors_, normalerweise um ein gemeinsames Set an Funktionen auf sie anzuwenden.

- `name`: Neuer gemeinsamer Name
- `value`: Neuer gemeinsamer Wert

## [`choose`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Choose.java)

Sendet nur einen Literal aus einer Gruppe weiter (mit abnehmender
Priorität).

- `name`: Name des neuen Literals
- `value`: Wert des neuen Literals

## [`range`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Range.java)

Wertet zwei aufeinanderfolgende Literale als ganze Zahlen aus, die den Start- und
Endpunkt einer Zahlenreihe definieren. Alle Zahlen dieser Reihe werden
anschliessend als Literale ausgegeben.

- `name` (obligatorisch): Name der neuen Literale
- `increment`: Inkrement (Standard: `1`)

## [`equalsFilter`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/EqualsFilter.java)

Literale werden weitergeleitet, sofern alle Werte gleich sind.

- `name` (obligatorisch): Name des neuen Literals
- `value` (obligatorisch): Wert des neuen Literals

## [`all`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/All.java), [`any`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/Any.java) & [`none`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/collectors/None.java)

Überprüfen, ob angegebenen Literale vorhanden sind. Geben standardmässig eine
unbenannte (änderbar mit `name=`) Variable mit `true` als Wert (änderbar mit
`value=`) aus, insofern die jeweilige Bedingung zutrifft:

- Bei `all`: Alle Literale sind im Datensatz vorhanden
- Bei `any`: Mindestens ein Literal ist im Datensatz vorhanden
- Bei `none`: Kein Literal ist im Datensatz vorhanden

{{% block note %}}
Die Verwendung der drei Quantoren macht insbesondere Sinn im Zusammenhang mit
[`if`-Anweisungen](#if-anweisungen).
{{% /block %}}
