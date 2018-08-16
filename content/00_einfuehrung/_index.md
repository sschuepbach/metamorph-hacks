---
title: "Einführung"
date: 2018-08-10T20:32:00+02:00
anchor: "einfuehrung"
weight: 1
---

Dieses README beinhaltet Informationen zu Funktionen und _collectors_ in
Metamorph sowie eine Sammlung von Ansätzen ("Hacks"), wie komplexere
Transformationsworkflows in Metamorph bewerkstelligt werden können.

## Generelle Hinweise zu Metamorph-Dateien

Metamorph-Dateien sind im XML-Format. Jede Metamorph-Regel muss innerhalb des
`<metamorph>`-Elementes abgelegt werden:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metamorph xmlns="http://www.culturegraph.org/metamorph" version="1">
  <!-- Hier kommen die Definitionen hin -->
</metamorph>
```

Direkt unterhalb des `<metamorph>`-Elementes können folgende Elemente
definiert werden:

- `<include>`: [Einbinden von externen Definitionen](#includes)
- `<macros>`: [Dateiweite Funktionen](#makros)
- `<maps>`: Liste von [Nachschlagetabellen](#nachschlagetabellen)
- `<meta>`: Metadaten der Datei: Gültige Kindelemente sind `<name>` und
  `<annotation>`
- `<rules>`: Tranformations- und Filterregeln (die den Hauptteil dieser
  Dokumentation ausmachen)
- `<vars>`: [Dateiweite Konstanten](#variablen)

Metamorph selbst hat zwei eigene Attribute:

- `version`: Die Version des Metamorph-Schemas
- `entityMarker`: Trennzeichen zwischen Entität und einem Literal
  (Standard: `.`)
