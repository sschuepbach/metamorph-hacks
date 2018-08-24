---
title: Nachschlagetabellen
date: 2018-08-13T13:39:00+02:00
anchor: nachschlagetabellen
weight: 14
---

Nachschlagetabellen dienen zur Filterung oder Ersetzung bestimmter Werte und
werden so von den Funktionen [`whitelist`]({{< ref "filter.md#whitelist" >}}) und [`blacklist`]({{< ref "filter.md#blacklist" >}}) sowie [`setreplace`]({{< ref "transformationen.md#replace-setreplace" >}}) und
[`lookup`]({{< ref "transformationen.md#lookup" >}}) verwendet. Sie können in Metamorph an verschiedenen Stellen definiert
werden:

- Wird die Nachschlagetabelle nur an einer einzigen Stelle benötigt, macht es
  Sinn, sie direkt in der Funktion __des betreffenden Literal selbst__ zu
definieren. Dies geschieht mithilfe des Enumerierungstags `entry`, welches das
obligatorische Attribut `name=` (Schlüssel) und das fakultative Attribut
`value=` (Ersetzungswert).
- Die vier erwähnten Funktionen haben auch ein fakultatives Attribut `map=`. Mit
  diesem kann eine __im gesamten Morph verfügbare Tabelle__ referenziert werden, die in einem vom
`<rules>`-Abschnitt separierten Tag `<maps>` definiert wird:

    ```xml
    <maps>
      <map>
         <entry name="schluessel1" value="wert1"/>
         <entry name="schluessel2" value="wert2"/>
      </map>
    </maps>
    ```
- Schliesslich kann ebenfalls im Tag `<maps>` analog zu `map` eine Reihe von __externen
  Quellen__ als Nachschlagetabelle angegeben werden.

### javamap
`javamap` referiert eine Java-Klasse, welche `java.util.Map` implementiert

- `name` (obligatorisch): Name der Tabelle in Morph-Datei
- `class` (obligatorisch): Klassenname mit vollem Pfad
- Alle weiteren Attribute werden als Parameter der Klasse
  interpretiert

### filemap
{{< icon name="fa-github" >}}[`filemap`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/maps/FileMap.java) referiert eine Textdatei, wobei in dieser eine Zeile einem Schlüssel-Wert-Paar entspricht

- `name` (obligatorisch): Name der Tabelle in Morph-Datei
- `files` (obligatorisch): Pfade zu Dateien mit Tabellen, getrennt durch
Leerzeichen
- `separator`: Trennzeichen zwischen Schlüssel und Wert in Tabelle
(Standard: `\t`)

### restmap
{{< icon name="fa-github" >}}[`restmap`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/maps/RestMap.java) fragt eine REST-Schnittstelle ab

- `name` (obligatorisch): Name der Tabelle in Morph-Datei
- `url` (obligatorisch): URL der REST-Schnittstelle

### sqlmap
{{< icon name="fa-github" >}}[`sqlmap`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/maps/SqlMap.java) fragt eine SQL-Datenbank ab

- `name` (obligatorisch): Name der Tabelle in Morph-Datei
- `host`: Hostname (Standard: `localhost`)
- `login` (obligatorisch): Benutzername 
- `password` (obligatorisch): Passwort
- `database` (obligatorisch): Name der Datenbank
- `query` (obligatorisch): Suchabfrage
- `driver`: Datenbanktreiber (Standard: `com.mysql.jdbc.Driver`)

### jndisqlmap
{{< icon name="fa-github" >}}[`jndisqlmap`](https://github.com/metafacture/metafacture-core/tree/master/metamorph/src/main/java/org/metafacture/metamorph/maps/JndiSqlMap.java) fragt eine SQL-Datenbank über die [_JNDI_](https://de.wikipedia.org/wiki/Java_Naming_and_Directory_Interface)-Schnittstelle ab

- `name` (obligatorisch): Name der Tabelle in Morph-Datei
- `datasource` (obligatorisch): Datenquelle
- `query` (obligatorisch): Suchabfrage
