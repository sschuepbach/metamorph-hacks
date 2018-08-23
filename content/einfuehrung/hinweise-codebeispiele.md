---
title: Hinweise zu Codebeispielen
date: 2018-08-23T14:44:00+02:00
anchor: hinweise-codebeispiele
weight: 3
---

## Formeta

In der Dokumentation finden sich immer wieder Codebeispiele. Ein- und
Ausgaben werden dabei im sog. [_Formeta_](http://b3e.net/metamorph-book/latest/datamodel.html)-Format dargestellt. Formeta hat lediglich zwei Bausteine: _Literale_ (Schlüssel-Wert-Paare) und _Entitäten_ (Objekte, die Literale oder weitere Entitäten enthalten). Weiter gelten folgende Regeln:

- Jeder Datensatz ist eine Entität
- Literale werden in der Form `schlüssel: wert` geschrieben und mit Kommata
  (`,`) voneinander getrennt
- Entitäten werden in der Form `schlüssel { <objektinhalt> }`
  geschrieben und können optional mit Kommata voneinander getrennt werden
(Achtung: Zwischen `schlüssel` und `{` hat es im Unterschied zu JSON keinen Doppelpunkt!)
- Mit `\` können Zeichen mit spezieller Bedeutung maskiert werden: Das sind
  neben den Trennzeichen `:`, `,`, `{` und `}` `'` (einfaches
Anführungszeichen) und das Maskierungszeichen `\` selbst.
  `:`.
- Anderseits haben `\n` (Zeilenumbruch) und `\r` (Wagenrücklauf)
  spezielle Bedeutung
- `:`, `,`, `{` und `}` müssen in Werten nicht maskiert werden, wenn der
  Text in einfachen Anführungszeichen steht
- Leerzeichen, Tabulatoren, Zeilenumbrüche und Wagenrückläufe können optional zur besseren Lesbarkeit eingesetzt werden

Ein Beispiel:

```
{
  kanton1 {
    name: Appenzell Ausserrhoden,
    hauptort: 'Herisau, Trogen',
    einwohnerzahl: 55\'000
  },
  kanton2 {
    name: Appenzell Innerrhoden,
    hauptort: 'Appenzell',
    einige berge: {
      hoher berg: säntis,
      und: schäfler
    }
}
```

Dasselbe Beispiel in kompakter Schreibweise ohne Leerzeichen,
Zeilenumbrüche und optionalen Kommata:

```
{kanton{name:Appenzell Ausserrhoden,hauptort:Herisau\, Trogen,einwohnerzahl:55\'000}kanton2{name:Appenzell Innerrhoden,hauptort:Appenzell,einige berge{hoher berg:säntis,und:schäfler}}}
```

## Metafacture Runner

Um die Beispiele selber durchspielen zu können, muss der sog.
_Metafacture runner_ (eine ausführbare Instanz von Metafacture)
installiert werden. Dazu die aktuelle Metafacture-Distribution [herunterladen](https://github.com/metafacture/metafacture-core/releases) und entpacken. Weitere Hinweise dazu finden sich auf der [Website von
Metafacture](https://github.com/metafacture/metafacture-core). Zudem ist
eine
sog. [Flux](https://github.com/metafacture/metafacture-core/wiki/Flux-user-guide)-Datei notwendig, in der der Transformationsworkflow definiert
wird. Eine minimale Flux-Datei, in der das obige Beispiel transformiert
wird, ist folgendermassen aufgebaut:

```
FLUX_DIR + "eingabe.formeta"|   // Pfad zur Eingabedatei; FLUX_DIR enthält den Pfad zur Flux-Datei
open-file|                      // Öffne Datei
as-formeta-records|             // Prozessiere Dateitext als Formeta
decode-formeta|                 // Dekodiere Formeta
morph(FLUX_DIR + "morph.xml")|  // Wende Transformationsregeln an (in morph.xml definiert)
encode-formeta|                 // Serialisiere als Formeta
write("stdout");                // Gib Resultat auf Kommandozeile aus
```

Nachdem _Metafacture runner_ installiert und die Flux- sowie die
Eingabedatei erstellt sind, ins Verzeichnis der Flux-Datei wechseln und auf
der Kommandozeile folgendes eingeben:

```bash
/pfad/zu/metafacture/runner/flux.sh flux.datei
```

## Docker-Image

Anstatt _Metafacture runner_ zu installieren, kann auch das
entsprechende
[Docker](https://docker.com)-Image verwendet werden. Dazu muss Docker lokal
installiert werden. Anschliessend über Kommandozeile ins Verzeichnis
wechseln, wo sich die Flux- und die Eingabedatei befinden, und eingeben:

```bash
docker run -v `pwd`:/mfwf:ro sschuepbach/metafacture-runner:latest flux.datei
```

{{% notice note %}}
Wird das Docker-Image zum ersten Mal ausgeführt, dauert der Prozess etwas
länger, da zuerst das entsprechende Image heruntergeladen werden muss.
{{% /notice %}}
