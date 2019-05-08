# CamundaBPM DMN Implementation

## Allgemeines
In dieser Dokumentation wird eine DMN Task über die Camunda BPM Plattform mittels der DMN Implementation ausgeführt. Dabei werden auf die einzelnen Schritte eingegangen die für die Implementierung relevant sind.

Die Dokumentation setzt voraus, dass die Camunda BPM Plattform und seine zusätzlichen Pakete installiert sind. Auch wird nicht näher erläutert wie andere Tasks (User-Task) innerhalb des BPMN-Modells zu konfigurieren ist. 

Sollten hier wissenslücken aufkommen, wird die Camunda BPM Dokumentation empfohlen: <https://docs.camunda.org/manual/7.10/>

Für eine breite exemplarische Implementierung wird das folgene Projekt empfohlen: https://github.com/strumswell/softwareauswahl-kgwebservice

## DRD

Dabei wurde ein DRD als Basis für fünf wesentliche Decisions erstellt. Die **Informationen** Decision erzeugt ein notwendiges Array um die Ausgabewerte der einzelnen Tabellen für die Übergabe innerhalb der Camunda BPM Engine zu ermöglichen. Im DRD sind neben der Decisions auch fünf Input Data Shapes sowie jeweils eine Business Knowledge Rule und eine Knowledge Source. Diese weiteren Shapes tragen keine technischen Konfigurationen und tragen dem besseren Verständnis bei, welcher logische Kontext hinter den einzelnen DMN Tabellen steht.

![alt text](https://github.com/MCikus/CamundaBPM-DMN-Implementation/blob/master/pictures/DMN.png?raw=true "DMN")

## DMN Tabellen

In diesem Abschnitt werden die DMN Tabellen kurz erläutert und vorgestellt. 

### Anzahl der Berater

In dieser Tabelle wird die Anzahl der benötigten Berater festgelegt. Diese Ergibt sich allein aus der zu bearbeiten Themenvielfalt. Für jedes neue zu bearbeitene Thema für einen Kunden wird ein Berater dazu gezogen. Bsp. IT, Logistik und Controlling ergeben 3 Themengebiete - also drei Berater, jedoch nicht mehr als sieben Berater. 

| Notwendige Berater |  |  |  |
|--------------------|----------------|---------------|------------|
| berateranzahlID |  |  |  |
| C+ | Input | Output | Annotation |
|  | Themenvielfalt | Berateranzahl |  |
|  | integer | double |  |
| 1 | >= 1 | 1 | - |
| 2 | >= 2 | 1 | - |
| 3 | >= 3 | 1 | - |
| 4 | >= 4 | 1 | - |
| 5 | >= 5 | 1 | - |
| 6 | >= 6 | 1 | - |
| 7 | >= 7 | 1 | - |

### Stunden

In dieser Tabelle werden die Stunden kalkuliert, welche sich je nach Komplexität, Deadline des Projektes und der Entfernung zum Kunden ergeben. Bei einer mittleren oder niedrigen Komplexität wird davon ausgegangen den Kunden auch Remote zu bedienen, was in der theorie Zeit einspart und sich direkt auf die Entscheidung auswirkt. 

| Benötigte Stunden |  |  |  |  |  |
|-------------------|--------------|----------------------------------------|------------|---------|------------|
| stundenID |  |  |  |  |  |
| U | Input |  |  | Output | Annotation |
|  | Komplexitaet | Deadline | Entfernung | Stunden |  |
|  | string | date | integer | double |  |
| 1 | "hoch" | < date and time("2019-08-04T00:00:00") | < 100 | 20 |  |
| 2 | "hoch" | > date and time("2019-08-03T00:00:00") | < 100 | 15 |  |
| 3 | "hoch" | < date and time("2019-08-04T00:00:00") | >= 100 | 30 |  |
| 4 | "hoch" | > date and time("2019-08-03T00:00:00") | >= 100 | 25 |  |
| 5 | "mittel" | - | - | 10 |  |
| 6 | "niedrig" | - | - | 5 |  |

### Beraterpreis 

In dieser Tabelle werden die Stundenpreise festgelegt die ein Berater für einen Kunden kostet. Je mehr Berater und Stunden desto geringer fällt der Stundenpreis aus. Die Auslastung des fiktiven Unternehmens wirkt sich dahingehend aus, dass bei einer hohen Auslastung ein Standardpreis von 80,00 EUR veranschlagt wird. Dabei spielt die Anzahl der Berater und die Stundenanzahl keine Rolle.

| Beraterpreis |  |  |  |  |  |
|--------------|---------------|---------|-------------|--------|------------|
| preisID |  |  |  |  |  |
| U | Input |  |  | Output | Annotation |
|  | Berateranzahl | Stunden | Ausgelastet | Preis |  |
|  | integer | integer | boolean | double |  |
| 1 | 1 | 5 | false | 50.00 | - |
| 2 | 2 | 5 | false | 52.00 | - |
| 3 | 3 | 5 | false | 54.00 | - |
| 4 | 4 | 5 | false | 56.00 | - |
| 5 | 5 | 5 | false | 58.00 | - |
| 6 | 6 | 5 | false | 60.00 | - |
| 7 | 7 | 5 | false | 62.00 | - |
| 8 | 1 | 10 | false | 49.00 | - |
| 9 | 2 | 10 | false | 51.00 | - |
| 10 | 3 | 10 | false | 53.00 | - |
| 11 | 4 | 10 | false | 55.00 | - |
| 12 | 5 | 10 | false | 57.00 | - |
| 13 | 6 | 10 | false | 59.00 | - |
| 14 | 7 | 10 | false | 61.00 | - |
| 15 | 1 | 15 | false | 48.00 | - |
| 16 | 2 | 15 | false | 50.00 | - |
| 17 | 3 | 15 | false | 52.00 | - |
| 18 | 4 | 15 | false | 54.00 | - |
| 19 | 5 | 15 | false | 56.00 | - |
| 20 | 6 | 15 | false | 58.00 | - |
| 21 | 7 | 15 | false | 60.00 | - |
| 22 | 1 | 20 | false | 47.00 | - |
| 23 | 2 | 20 | false | 49.00 | - |
| 24 | 3 | 20 | false | 51.00 | - |
| 25 | 4 | 20 | false | 53.00 | - |
| 26 | 5 | 20 | false | 55.00 | - |
| 27 | 6 | 20 | false | 57.00 | - |
| 28 | 7 | 20 | false | 59.00 | - |
| 29 | 1 | 25 | false | 46.00 | - |
| 30 | 2 | 25 | false | 48.00 | - |
| 31 | 3 | 25 | false | 50.00 | - |
| 32 | 4 | 25 | false | 52.00 | - |
| 33 | 5 | 25 | false | 54.00 | - |
| 34 | 6 | 25 | false | 56.00 | - |
| 35 | 7 | 25 | false | 58.00 | - |
| 36 | 1 | 30 | false | 45.00 | - |
| 37 | 2 | 30 | false | 47.00 | - |
| 38 | 3 | 30 | false | 49.00 | - |
| 39 | 4 | 30 | false | 51.00 | - |
| 40 | 5 | 30 | false | 53.00 | - |
| 41 | 6 | 30 | false | 55.00 | - |
| 42 | 7 | 30 | false | 57.00 | - |
| 43 | - | - | true | 80.00 | - |

### Gesamtpreis 

In dieser Literal Expression werden die erzeugten Ausgabewerte innerhalb der DMN Engine ausmultipliziert und es ergibt sich der Gesamtpreis aus den Stunden, der Berateranzahl und des kalkulierten Stundenpreises. Die Literal Expression umgeht eine zu große und unübersichtliche Tabelle.

![alt text](https://github.com/MCikus/CamundaBPM-DMN-Implementation/blob/master/pictures/gesamtPreis.png?raw=true "LiteralExpression1")

### Ausgabewerte

In dieser Literal Expression wird ein Array erstellt, welche dazu dient alle Ausgabewerte innerhalb der DMN Engine in der BPMN Implementierung verfügbar zu machen. Es ist wichtig hierbei Groovy als Expression Language auszuwählen. 

![alt text](https://github.com/MCikus/CamundaBPM-DMN-Implementation/blob/master/pictures/InformationsListe.PNG?raw=true "LiteralExpression2")
