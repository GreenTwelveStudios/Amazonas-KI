---
layout: default
image_sliders:
   - BeobachterSlider
   - GameSlider
---

Willkommen auf unserer Website GreenTwelveStudios.

Für das Spiel „Amazonen“ haben wir eine KI im Rahmen des Software-Technik Praktikums 20/21 entwickelt, auf die wir im folgenden näher eingehen möchten.

## Neuronales Netz

![Neuronales Netz](/img/NeuronalesNetzWeiss.png)

Unsere KI berechnet Züge auf der Basis eines neuronalen Netzes. Ein Neuronales Netz lässt sich als gerichteter gewichteter Graph darstellen. Dieser Graph lässt sich in verschiedene Schichten (layer) unterteilen. Es gibt jeweils ein input, und output layer, sowie beliebig viele hidden layer. Die Werte aller eingehenden Kanten eines Knotens werden bei der berechnung addiert und auf das Ergebnis wird eine Aktivierungsfunktion (wir nutzen hierfür die Sigmoidfunktion) angewendet. Dies dient als output für alle ausgehenden Kanten des Graphen.

![Sigmoidfunktion](/img/Sigmoid.png)


## Architektur des KI-Spielers und des KI-Trainings

![Klassendiagramm KI-Spieler](/img/KI-KlassenSpieler.png) ![Klassendiagramm KI-Training](/img/Ki-Training_Klassen.png)


Der Manager ist die Zentrale Klasse des KI-Spielers, er verwaltet die verschiedenen Spiele. Außerdem ist er bei der Berechnung eines Zuges dafür zuständig, die einhaltung der MaxTurn-Time  sowie  die  Korrektheit  des  berechneten  Zuges  zu  überprüfen.Der AIController  ist  die  Schnittstelle  zum  Neuronalen Netz, welches die eigentliche Berechnung durchführt. Die Klasse AIControllerCallable dient als Wrapper Klasse um das berechnen in einem Thread zu ermöglichen. Außerdem gibt es die Klasse RandomTurn, welche einen zufälligen aber immer korrekten Zug zurückgibt.

Der TrainingManager überwacht als zentrale Klasse das Training  und  ist  für  das  erstellen  der  Games,  den  import/export sowie für das weiterentwickeln und verändern am Ende einer Generation zuständig (mehr dazu unten).Ein einzelnes Game wird vom GameController verwaltet, welcher für das verteilen der Rewards und überwachen der Züge zuständig ist. Die Berechnungen der jeweiligen KI-Spieler werden vom AIControllerTraining, einem speziell für das Training angepassten AIController, durchgeführt.(Abb. KI TRAININGS KLASSE)


## KI-Spieler

Unser KI-Spieler besitzt zwei Möglichkeiten einen Zug zu berechnen: Die erste und die voraussichtlich am meisten genutzte Möglichkeit ist ein neuronales Netz, welches vorher mit Hilfe der KI-Training Komponente trainiert wurde und vom KI-Spieler importiert werden kann.
Falls die Berechnung des neuronalen Netzes zu lange dauert oder der berechnete Zug falsch ist, wird ein mit Hilfe eines zufälligen Verfahrens berechneter Zug verwendet. Die Berechnung des neuronalen Netzes geschieht in einem eigenen Thread, damit die analytische Berechnung gleichzeitig erfolgen kann und außerdem so ein Timeout einfach realisiert werden kann.


## KI-Training

Die Aufgabe des KI-Trainings ist es ein Neuronales Netz zu trainieren, sodass dieses möglichst gute Züge macht. Dies geschieht mit Hilfe von Deep Reinforcement Learning.
Eine Generation wird dabei wie Folgt trainiert:

![Trainingsablauf](/img/KI-Training.png)

Zuerst spielt für jedes übergebene Board, jede KI gegen jede andere KI, alternativ kann eine KI auch gegen zufällige Züge spielen (um das korrekte Ziehen zu trainieren). Für jedes Spiel werden bestimmte Rewards für jede KI vergeben (abhängig von Sieg/Niederlage, falscher/richtiger Zug, ...), diese Punkte können frei konfiguriert werden und sich von Generation zu Generation unterscheiden. Sind nun alle Spiele abgeschlossen, so werden die besten KIs ausgewählt und zu der nächsten Generation hinzugefügt. Danach werden mithilfe der besten KIs Nachfolger erzeugte, indem die Struktur des Netzes oder die Gewichte mutieren. Die KIs werden nun dadurch besser, dass immer die besten KIs einer Generation ausgewählt werden und diese sich weiterentwickeln können, um eventuell eine noch bessere Leistung zu erzielen. Passiert dies ersetzt die neue KI eine schwächere und pflanzt sich stattdessen fort bzw. entwickelt sich weiter.


## Beobachter

anbei Einige Bilder zu unserem Beobachter und seinen Funktionen:

{% include slider.html selector="BeobachterSlider" %}

Hier ist noch ein Beispielspiel unserer KI zu sehen:

{% include slider.html selector="GameSlider" %}
