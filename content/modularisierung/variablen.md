---
title: Variablen
date: 2018-08-13T17:30:00+02:00
anchor: variablen
weight: 32
---

Werte in Metamorph sind bis zu einem gewissen Grad dynamisierbar, indem
sie als Parameter in der Flux-Datei oder über die Kommandozeile dem
Programm übergeben werden. Dazu ist es notwendig, dass in der Flux-Datei
das `morph`-_command_ so angepasst wird, dass solche Parameter
automatisch der `Metamorph`-Klasse übergeben werden:

```
morph(morphdatei, *)
```

Anschliessend können arbiträre Parameter in Flux definiert werden:

```
default var1 = "wert1";
default var2 = "wert2";
// Flux-Commands...
```

`default` verhindert, dass bereits auf der Kommandozeile definierte
Parameter überschrieben werden. Letzteres kann folgendermassen
bewerkstelligt werden:

```
./flux.sh fluxdatei var1=wert1 var2=wert2
```

In Metamorph werden so übergebene Parameter mittels `$[variablenname]` etc.
ausgelesen, also beispielsweise:

```xml
<data source="_id" name="feldA">
  <constant value="$[var1]"/>
</data>
```

Selbstredend können solche Variablen überall dort in
Metamorph-Definitionen verwendet werden, wo auch Werte definiert werden können.

Schliesslich lassen sich in Metamorph auch noch Standardwerte für
Variablen definieren. Dies geschieht in einem vom `<rules>`-Element
separierten `<vars>`-Tag:

```
<vars>
  <var name="var1" value="wert1"/>
  <var name="var2" value="wert2"/>
</vars>
<!-- <rules> ... -->
```
