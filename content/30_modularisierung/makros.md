---
title: Makros
date: 2018-08-13T17:30:00+02:00
anchor: makros
weight: 33
---

Noch einen Schritt weiter als Variablen hinsichtlich der
Wiederverwendbarkeit gehen Makros. Mit ihnen lassen sich
eigentliche parametrisierbare Funktionen innerhalb der Morph-Datei definieren. Wie Variablen werden sie ausserhalb des `<rules>`-Elementes definiert, in diesem Fall im `<macros>`-Tag:

```xml
<macros>
  <macro name="eindeutiger_name">
  <!-- Makro -->
  </macro>
  <!-- Weitere Makros -->
</macros>
<!-- <rules> ... -->
```

Ein Makro muss zwingend mit einem eindeutigen Namen (`name=`) versehen werden, damit es später im `<rules>`-Block aufgerufen werden kann.

Makros können einen beliebigen Block an Metamorph-Definitionen
enthalten, also Literale mit ihren Funktionen, _collectors_ sowie eine Kombination davon. Parameter werden wie Variablen in eckigen Klammern mit führendem Dollarzeichen geschrieben (`$[param1]`). Ein Beispiel eines einfachen Makros:

```xml
<macro name="makro1">
  <concat delimiter=" " name="$[ccName]" flushWith="$[abschickenMit]">
    <data source="$[dsSource]" >
      <case to="upper"/>
    </data>
  </concat>
</macro>
```

Makros werden mit dem Element `<call-macro>` aufgerufen. `call-macro` umfasst
ein obligatorisches Attribut, der Name des Makros (`name=`). Alle weiteren
Attribute werden als Argumente des Makros interpretiert. Das obige Makro wird also beispielsweise folgendermassen aufgerufen>

```xml
<combine name="name" value="${vorname} ${nachname}">
  <call-macro name="makro1" ccName="vorname" dsName="feldA" abschickenMit="nachname" />
  <data source="feldB" name="nachname"/>
</combine>
```
