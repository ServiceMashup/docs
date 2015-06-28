#Analyse, Design & Architektur

##Ziele

Auch wenn wir uns über die Entwicklung neuer Anwendungen von der "grünen Wiese" weg freuen, so wenig ist der Begriff aus Sicht der Unternehmung passend. In den meisten Organisationen existieren vorhandene oder gewünschte Strukturen. Mit zunehmender von innen oder außen gewünschter organisatorischer Dynamik ändern sich stetig Wünsche, Struktur, Organisationskommunikation oder Zuständigkeiten. Das Problem ist somit weniger die heutig bekannte Struktur, sondern die Fähigkeit auf morgige Anforderungen schnell, kostengünstig und verlässlich zu reagieren. 

##Analyse und Entwurf

Mit Microservice-Architekturen sollen Anwendungen entstehen, die dynamische Geschäftsmodelle unterstützen und Anforderungen von morgen leicht und flexibel technisch unterstützen. Dabei liegt der Schlüssel einer effizienten Anforderungsanalyse im Zerlegen von großen Aufgaben in kleinere Aufgaben. In den meisten Fällen spiegeln Organisationen diese Art der Aufgabenverteilung bzw. Zuständigkeiten und es etablieren sich mehr oder minder starre Kommunikationsstrukturen und Prozesse zwischen ihnen. 

Oft ist der Blick auf die vorhandene oder gewünschte Organisationstruktur, dessen Kommunikation und Prozesse ein gutes Mittel das Bild auf die Architektur zu schärfen. Ein erster Entwurf sollte sich somit auf die unterschiedlichen Rollen bzw. Abteilungen und dessen spezifische Domäne (DDD auch Bounded Context genannt) und Begriffe auseinandersetzen. In der genaueren Analyse einer spezifischen Domäne sollten Sie Zuständigkeiten, Begriffe, Prozesse und die Kommunikation im innerin und nach außen erarbeiten. Nicht sellten können Sie innerhalb einer Domäne abermals Teilaufgaben erkennen. Die meist vorhandene, technisch neutrale hierarchische Struktur von Aufgaben, Teilaufgaben und Teil-Teilaufgaben ist meist ein guter Anfang für einen ersten Architekturentwurf.

An dieser Stelle kann ich den interessierten Leser wärmstens Techniken und Praktiken aus [DDD](https://de.wikipedia.org/wiki/Domain-Driven_Design) empfehlen. 

Für unsere zukünftige Anwendung ergibt sich nach einer ersten Analyse folgendes Organigramm.

![Organisationsstruktur](images/organisation-structure.png)

Wem es nicht gleich gelingt einen ersten Entwurf in angemessener Zeit aus der Vogelperspektive zu erarbeiten, kann sich auf einen Teilbereich konzentrieren. Ein Entwurf, wie der Begriff bereits prägt, hat keinesfalls den Anspruch auf Vollständigkeit. Vielmehr ist die Erarbeitung innerhalb eines angemessenen Zeitfensters entscheidend. Nacharbeit und möglicherweise verbundenes Refaktorieren sollte bewusst in Kauf genommen werden. Schrittweises Erarbeiten und Anpassung durch Erkenntnissen aus der Fachdomäne soll Sie und Ihre Kollegen vor bewussten oder unbewussten Annahmen, oder schlimmer Umsetzungen, schützen. 

##Architektur

![Architektur](images/architecture.png)


