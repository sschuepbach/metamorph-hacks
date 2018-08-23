---
title: "Homepage"
date: 2018-08-23T14:48:00+02:00
anchor: "homepage"
---

Zur Datentransformation und zum Datenmapping im Rahmen des Projektes
[_linked-swissbib_](https://github.com/linked-swissbib) verwenden wir fast
ausschliesslich das Framework [Metafacture](https://github.com/metafacture).
Da dieses Feld durchaus seine Schwierigkeiten und Tücken hat, ist auch die
Handhabung des Frameworks nicht ganz einfach. Insbesondere der "Kern" von
Metafacture, __Metamorph__, mit dem die Transformations- und Mappingregeln
definiert werden, ist anspruchsvoll.  Auf Grundlage unserer Erfahrungen haben
wir die vorliegende Dokumentation geschrieben.

Im ersten Teil werden die Grundlagen von Metamorph vorgestellt: Bearbeitung
einzelner _Literale_ (Schlüssel-Wert-Paare), Prozessierung von
Literalverbünden (_collectors_) sowie die Wiedernutzung von einzelnen
Literalen und Morph-Definitionen. Der Inhalt orientiert sich an der
[offiziellen
Metafacture-Dokumentation](https://github.com/metafacture/metafacture-core/wiki)
und am [XML-Schema von
Metamorph](https://github.com/metafacture/metafacture-core/blob/master/metamorph/src/main/resources/schemata/metamorph.xsd),
versucht aber auch, einzelne Konzepte, die uns nicht auf Anhieb klar waren,
genauer zu beleuchten. 

{{% notice info %}}
Hinweise zu weiteren Metafacture-Ressourcen finden sich im
[Anhang]({{< ref "anhang/ressourcen.md" >}})
{{% /notice %}}

Da wir aus unseren Erfahrungen mit linked-swissbib gelernt haben, dass die korrekte
Anwendung von Metamorph-Regeln in Einzelfällen tückisch sein kann, werden im
zweiten Teil in Form eines "Kochbuches" "Rezepte" für einzelne
Problemstellungen vorgestellt. Es versteht sich von selbst, dass dieser Teil
keinen Anspruch auf Vollständigkeit erhebt. Ergänzungen sind natürlich
herzlich willkommen. 

Im Anhang schliesslich finden sich neben einem Glossar und einer Liste von
Ressourcen zu Metamorph/Metafacture im Netz auch eine Besprechung der in
linked-swissbib verwendeten Metamorph-Definitionen. Dieser Abschnitt stellt
eine Ergänzung zu den in den ersten beiden Teilen vorgestellten Inhalten dar.
