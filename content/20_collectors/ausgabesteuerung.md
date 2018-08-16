---
title: "Ausgabesteuerung"
date: 2018-08-10T22:24:00+02:00
anchor: "ausgabesteuerung"
weight: 22
---

Wann und wie _collectors_ die gesammelten Werte weiterschicken, kann mit
Hilfe von drei Parametern definiert werden: `flushWith`, `reset` und
`sameEntity`.

`flushWith` definiert den Zeitpunkt, zu welchem der _collector_ die Werte
abschickt (_flush_). Dies kann entweder nach der Verarbeitung eines bestimmten Feldes (`flushWith="feldname"`, Wildcards erlaubt) oder des gesamten Datensatzes sein (`flushWith="record"`). Im Fall von `combine`,  `entity`, `equalsFilter` sowie den Quantoren `all`, `any` und `none` werden die gesammelten Werte standardmässig weitergeleitet, sobald für jedes Feld ein Wert vorhanden ist. 

{{% block warn %}}
In einem _collector_ gesammelte Werte werden __nicht__ weitergeleitet, wenn
die `flushWith`-Bedingung nicht eintritt. Dies ist der Fall, wenn

- der angegbene Feldname nicht vorhanden ist oder
- bei Standardeinstellung von `combine`, `entity`, `equalsFilter`, `all`, `any` und `none` der _collector_ nicht gefüllt werden kann
  _collector_ bis Ende des Datensatzes nicht gefüllt wird

Enthält ein _collector_ keine Werte bei Eintreffen der
`flushWith`-Bedingung, passiert ebenfalls nichts.
{{% /block %}}

`reset` löscht alle gesammelten Werte, nachdem diese weitergeleitet wurden.
Ist `reset="false"`, werden also nach einem erstmaligen _flush_ die aktuell
im _collector_ gespeicherten Werte bei __jedem__ neu gesammelten Wert
von Neuem abgeschickt!

`sameEntity` leitet nur Werte weiter, die in derselben Literalgruppe (_entity_) gesammelt wurden. Ist die Literalgruppe fertig prozessiert, ohne dass für jedes Feld ein Wert vorhanden ist, werden die gespeicherten Werte weggeworfen. Der _reset_-Mechanismus wird anderseits bei `sameEntity="true"` unabhängig von der Einstellung des `reset`-Paraemters ausgelöst, konnte die _flush_-Bedingung in einer Literalgruppe erfüllt werden. 

{{% block warn %}}
Im Fall von `combine` und `entity` überschreiben neue ältere Werte des
gleichen Datenfeldes.
{{% /block %}}

## Standardeinstellungen für `flushWith`, `reset` und `sameEntity`

Collector          |  flushWith       |  reset   |  sameEntity
------------------ | ---------------- | -------- | ------------
__`combine`__      | wenn vollständig | `false`  | `false`
__`concat`__       | `record`         | `true`   | `false`
__`entity`__       | wenn vollständig | `false`  | `false`
__`squares`__      | `record`         | `true`   | entfällt
__`tuples`__       | `record`         | entfällt | entfällt
__`group`__        | entfällt         | entfällt | entfällt
__`choose`__       | `record`         | `true`   | `false`
__`range`__        | `record`         | `false`  | `false`
__`equalsFilter`__ | wenn vollständig | `false`  | `false`
__`all`__          | wenn vollständig | `false`  | `false`
__`any`__          | wenn vollständig | `false`  | `false`
__`none`__         | wenn vollständig | `false`  | `false`
