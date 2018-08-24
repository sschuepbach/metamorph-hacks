---
title: Metamorph
date: 2018-08-23T14:47:00+02:00
anchor: metamorph
weight: 2
---

Innerhalb von Metafacture nimmt Metamorph die Rolle einer DSL
("Domain-specific language") ein, mit welcher einzelne Schlüssel-Wert-Paare
(sogenannte _Literale_) von Datensätzen transformiert oder gefiltert werden. Dafür werden Regeln (_"Metamorph"_- oder kurz _"Morph-Definition"_) festgelegt, welche anschliessend durch Metafacture auf einzelne Felder
(identifiert durch Schlüssel-/Feldnamen) angewendet werden. Diese Regeln
sind als XML-Elemente formuliert, und daher sind auch Metamorph-Dateien "normale" XML-Dateien.

{{% notice note %}}
Einen umfassenden Überblick über die erlaubten Elemente gibt das
[Metamorph-Schema](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/resources/schemata/metamorph.xsd).
{{% /notice %}}

## XML-Struktur

Jede Morph-Definition muss innerhalb des `<metamorph>`-Elementes abgelegt
werden:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metamorph xmlns="http://www.culturegraph.org/metamorph" version="1">
  <!-- Hier kommen die Definitionen hin -->
</metamorph>
```

Direkt unterhalb des `<metamorph>`-Elementes können folgende Elemente
definiert werden:

- `<include>`: [Einbinden von externen Definitionen]({{< ref
  "/modularisierung/includes.md" >}})
- `<macros>`: [Dateiweite Funktionen]({{< ref
  "/modularisierung/makros.md" >}})
- `<maps>`: Liste von [Nachschlagetabellen]({{< ref "/literale/nachschlagetabellen.md" >}})
- `<meta>`: Metadaten der Datei: Gültige Kindelemente sind `<name>` und
  `<annotation>`
- `<rules>`: Tranformations- und Filterregeln (die den Hauptteil dieser
  Dokumentation ausmachen)
- `<vars>`: [Dateiweite Konstanten]({{< ref
  "/modularisierung/variablen.md" >}})

Metamorph selbst hat zwei eigene Attribute:

- `version`: Die Version des Metamorph-Schemas
- `entityMarker`: Trennzeichen zwischen Entität und einem Literal


## Verwendung von Metamorph-Definitionen in Metafacture

In Metafacture werden Workflows mithilfe von sogenannten [_Flux_](https://github.com/metafacture/metafacture-core/wiki/Flux-user-guide)"-Skripten festgelegt. Flux besteht aus einer Reihe von Befehlen (_commands_), die jeweils einen Teil des Workflows beschreiben (bspw. das Öffnen einer Datei oder das Dekodieren eines Formates).

{{% notice note %}}
Ein Überblick der zur Verfügung stehenden _Flux_-Befehle ist [hier](https://github.com/linked-swissbib/mfWorkflows/tree/removed-mfrunner) zu finden.
{{% /notice %}}

Um eine Morph-Definition in einem solchen Flux-Skript zu verwenden, wird
der Befehl `morph('/pfad/zur/morph/datei')` benutzt. 

Daneben gibt es noch einen zweiten Befehl, der ebenfalls mit
Morph-Definitionen arbeitet: `filter('/pfad/zur/morph/datei')`. In einer
solchen Morph-Datei werden Regeln definiert - meistens das Vorhandensein einzelner
Felder -, welche von einem Datensatz erfüllt werden müssen, damit er im
Workflow weitergeleitet wird. Vielfach macht es Sinn, einen solchen Filter
mit einer nachfolgenden Transformation zu kombinieren, so beispielsweise
um zu vermeiden, dass Datensätze in die Transformation geschickt werden,
welche nicht "regelkonform" sind und daher zu unerwarteten Resultaten führen
können.

{{% notice note %}}
Ein Beispiel für einen solchen Fall ist die "baseline", ein
Datenworkflow in linked-swissbib, wo die Existenz eines gewissen Feldes -
MARC-Feld `245*.a` - Voraussetzung dafür ist, dass weitere Felder eines
Datensatzes im
Morph-Schritt korrekt zusammengefügt werden.
{{% /notice %}}
