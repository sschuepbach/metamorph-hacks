---
title: Includes
date: 2018-08-13T17:31:00+02:00
anchor: includes
weight: 34
---

Um Code-Blöcke nicht nur innerhalb einer Morph-Datei
wiederzuverwenden, sondern auch zwischen Dateien zu teilen, können
schliesslich sogenannte _XML inclusions_ (oder kurz [_XInclude_](https://www.w3.org/TR/xinclude/)) verwendet werden. Solche _inclusions_ sind möglich direkt im `<metamorph>`-Element, aber auch als Kind-Elemente von `<rules>`, `<maps>` und `<macros>`. Das heisst also, einfügbare Bausteine können Definitionen, Mappings, Makros oder der vollständige Inhalt einer Morph-Datei sein. 

Bausteine und die sie verwendenden Morph-Definitionen sind zur Laufzeit
eine einzige Einheit. Somit können beispielsweise Werte im Block in eine
Rekursion geschickt werden und in der Hauptdatei wieder ausgelesen,
oder in einem Block definierte Nachschlagetabellen in der Morph-Datei
aufgerufen werden.

{{% notice note %}}
Aus diesem Grund haben Blöcke nie ein `<metamorph>`- und, werden sie direkt
in einem der Kindelemente verwendet, auch kein `<rules>`-, `<maps>`- oder
`<macros>`-Element.
{{% /notice %}}

Eingefügt werden _XML inclusions_ folgendermassen:
```xml
<include href="pfad/zum/einzufuegenden/block.xml" parse="xml"
	 xmlns="http://www.w3.org/2001/XInclude" />
```

Während der Block beispielsweise so aussehen kann:
```xml
<!-- block.xml -->
<?xml version="1.1" encoding="UTF-8"?>
<map name="hauptorte" xmlns="http://www.culturegraph.org/metamorph">
  <entry name="Aargau" value="Aarau"/>
  <entry name="Appenzell Ausserrhoden" value="Herisau"/>
  <entry name="Appenzell Innerrhoden" value="Appenzell"/>
  <entry name="Basel-Landschaft" value="Liestal"/>
	<entry name="Basel-Stadt" value="Basel" />
  <!-- ... -->
</map>
```
