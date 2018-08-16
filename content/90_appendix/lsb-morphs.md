---
title: "Morphs in linked-swissbib"
date: 2018-08-13T07:53:00+02:00
anchor: "lsb-morphs"
weight: 91
---

## [documentMorph.xml](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/documentMorph.xml)

Erstellt Metadaten der Aufnahme. Pro `bibliographicRecord`-Ressource sollte
auch eine `document`-Ressource vorhanden sein.

## [enrichedMorph.xml](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/enrichedMorph.xml)

Verarbeitet Datensätze im _NTriples_-Format und transformiert sie zu
_JSON-LD_. Datensatz-Typen sind "Organisationen" (siehe Anmerkungen dazu im Abschnitt "organisationMorph.xml") und "Personen". Aufgrund der Syntax-Spezifika von _NTriples_ sowie für die
Normalisierung der Inputdaten, welche aus heterogenen Quellen stammen
(DBPedia, VIAF, vorverarbeitete Swissbib-Daten), müssen die meisten Werte
modifiziert werden. Die drei wichtigsten Fälle, die jeweils in einem
`<macro>`-Element definiert werden:

- Makro `year`: Jahresangaben sind immer nur auf das (vierstellige) Jahr genau, und nicht etwa in der Form `yyyy-mm-dd`.
- Makro `uri`: DBPedia-Links sollen immer zu `https://`-Links
  umgewandelt werden
- Makro `language-tag`: Das Sprach-Tag, welches in _NTriples_ mit einem
  `@`-Trennzeichen an das Literal angefügt wird, wird abgetrennt und in ein
eigenes Literal (`lang` überführt).

Personen und "Organsationen" haben verschiedene JSON-LD-Kontexte. Daher
gibt es zwei `<combine>`-Blöcke zur Erstellung des jeweiligen Kontextes, wo überprüft wird, ob es sich
um einen Datensatz vom Typ `Person` oder `Organization` handelt. Je nach
dem werden im betreffenden Kontext-Block die Daten weitergeleitet oder nicht.

## [itemMorph.xml](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/itemMorph.xml)

Erstellt Exemplardaten primär basierend auf dem nicht-kanonischen MARC-Feld
`949`. Hinweise:

- Die ID ist eine Kombination aus einem konstanten Präfix -
  `https://data.swissbib.ch/item/` -, dem Namen des Verbundes
(Information aus Unterfeld `949??.B`) sowie einem eindeutigen Hash-Wert,
der auf verschiedenen Werten aus den `949`er-Unterfeldern beruht. Der
Hashwert wird mit der Klasse [`ItemHash`](https://github.com/linked-swissbib/swissbib-metafacture-commands/blob/master/src/main/java/org/swissbib/linked/mf/morph/functions/ItemHash.java) erstellt.
- In einer bibliographischen Aufnahme können selbstverständlich Daten zu
  mehreren Exemplaren vorhanden sein. Dazu wird ein spezielles Verfahren
genutzt, welches Entitäten erstellt und in einem weiteren _command_ diese
zu eigenen _records_ umwandelt. Für Details siehe Hinweis-Block unten.
- Der `<combine>`-Block, wo der Hashwert generiert wird, befindet sich
  ausserhalb des `<entity>`-Blocks. Grund dafür ist das spezielle
Verhalten von Rekursionen, welche innerhalb von _collectors_
initialisiert werden. Für Details siehe Hinweise im [entsprechenden
Kapitel](#rekursionen).
- In einem weiteren `<combine>`-Element, `foaf:page`, wird ein Link zum
  Katalogisat im jeweiligen Katalog erstellt. Dazu werden verschiedene Werte
an eine weitere Klasse zur Verarbeitung geschickt, [`ItemLink`](https://github.com/linked-swissbib/swissbib-metafacture-commands/blob/master/src/main/java/org/swissbib/linked/mf/morph/functions/ItemLink.java).

{{% block info %}}
Ein wiederholt genutzte Methode für Extraktion von mehreren
eigenständigen Ressourcen aus einer einzigen bibliographischen (Eingabe-)Ressource
ist die Erstellung von Entitäten (`entity`) in der betreffenden Morph-Datei.
Dieser `entity`-Block umfasst alle Literale, welche für die jeweilige
Zielressource benötigt werden. Nach dem `morph`-_command_ folgt
[`entity-splitter`](https://github.com/linked-swissbib/swissbib-metafacture-commands/blob/master/src/main/java/org/swissbib/linked/mf/pipe/EntitySplitter.java), das einzelne Entitäten eines Records einliest, diese aber als
eigentständige Records wieder ausgibt. Mithilfe des Parameters
`entityBoundary` lässt sich zudem für mehrfach geschachtelte Entitäten einstellen, ab welcher Stufe die Entitäten Records werden sollen.
{{% /block %}}

## [organisationMorph.xml](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/organisationMorph.xml)

Erstellt "Organisations"-Entitäten (Organisationen, Institutionen, Kongresse
etc.) auf Basis von Marc-Feld
[`110`](https://www.loc.gov/marc/bibliographic/concise/bd110.html),
[`111`](https://www.loc.gov/marc/bibliographic/concise/bd111.html),
[`710`](https://www.loc.gov/marc/bibliographic/concise/bd710.html) oder
[`711`](https://www.loc.gov/marc/bibliographic/concise/bd711.html).

- Um solche Entitäten zu erstellen, wird der `entity`-_collector_ verwendet.
  Solche Unter-Entitäten werden dann im nächsten Metafacture-Schritt zu
eigenen Records "aufgewertet". Für Details siehe Hinweis-Block in
Abschnitt "itemMorph.xml".
- Da der Identifier analog zu `resourceMorph.xml` und `organisationMorph.xml`
  mit dem Hash-Mechanismus erstellt wird, ist es für Entitäten aus den Feldern
`110` und `111` zwingend, dass sie erst mit Unterfeld
[`245*.a`](https://www.loc.gov/marc/bibliographic/concise/bd245.html)
weitergeleitet werden.
- Damit eine Entität für Feld `110` oder `111` nicht "aus Versehen"
  weitergeleitet wird (sprich: es gibt in der Ressource gar kein
entsprechendes Feld), ist es wichtig, dass alle Bestandteile des
_collectors_ nur getriggert werden, wenn tatsächlich ein entsprechendes
(Unter-)Feld vorhanden ist. Denn wie im Kapitel
"[Ausgabesteuerung](#ausgabesteuerung)" beschrieben, wird ein _collector_ auch bei einem expliziten `flushWith` nur weitergeleitet, wenn er nicht leer ist. Entsprechend abgesichert wird an verschiedenen Stellen: Alle Literale (auch Konstanten) haben als Source `110??`, `111??` oder eines ihrer Unterfelder; dasselbe gilt für Literale in Kind-_collectors_; schliesslich wird bei der Hashwert-Generierung in [`authorHash110`](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/morphModules/authorHash110.xml) und [`authorHash111`](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/morphModules/authorHash111.xml) mit einer `if`-Anweisung getetest, ob entsprechende Unterfelder vorhanden sind - ansonsten wird kein Hash (auch kein `NO_HASH`!) generiert.
- Dieselbe Vorsichtsmassnahme ist für Entitäten der Felder `710` und `711`
  nicht notwendig, da sie mit dem demselben Feld getriggert werden, aus dem
sie ihre Werte beziehen.

## [personMorph.xml](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/personMorph.xml)

Erstellt Personen-Entitäten auf Basis von MARC-Feld [`100`](https://www.loc.gov/marc/bibliographic/concise/bd100.html) (1.
Indikator `0` oder `1`) oder [`700`](https://www.loc.gov/marc/bibliographic/concise/bd700.html) (1. Indikator dito). Der grundsätzlich Mechanismus ist derselbe wie bei `itemMorph.xml` und `organisationMorph.xml`: Literale werden pro relevantem Feld in `entity`-Blöcke zusammengefasst und anschliessend zu eigenständigen Ressourcen umgewandelt (für genauere Erläuterungen dazu s. Info-Box im Abschnitt zu `itemMorph.xml`). Zudem sind dieselben "Sicherheitsmassnahmen" wie bei `organisationMorph.xml` für Feld `100`-Fälle implementiert, um ein unabsichtliches Weiterleiten einer Entität ohne Werte aus dem entsprechenden Feld zu verhindern.

## [resourceMorph.xml](https://github.com/linked-swissbib/mfWorkflows/blob/removed-mfrunner/transformation/indexWorkflows/resourceMorph.xml)

Erstellt das eigentliche Katalogisat und linkt (bzw. wird verlinkt durch)
die Ressourcen `Person`, `Organisation`, `Document`, `Item` sowie die separat
erstellte `Work`.
