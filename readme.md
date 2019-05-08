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

In dieser Literal Expression wird ein Array erstellt, welche dazu dient alle Ausgabewerte innerhalb der DMN Engine in der BPMN Implementierung verfügbar zu machen. Es ist wichtig hierbei Groovy als Expression Language auszuwählen. Das DRD schließt durch diese Literal Expression mit nur mit der Variable `infoList` und trägt durch `infoList.addAll(tableVar)` alle benötigten Informationen. DMN und BPMN sind in der Camunda BPM Engine durch eine lose Kopplung sehr unabhängig voneinander. Mehr dazu im Abschnitt der folgenden BPMN Implementierung.

![alt text](https://github.com/MCikus/CamundaBPM-DMN-Implementation/blob/master/pictures/InformationsListe.PNG?raw=true "LiteralExpression2")

## BPMN Implementierung und Konfigurationen

Dieser Prozess ist auf das nötigste beschränkt und nur zur Eingabe von Daten innerhalb eines Formulars bzw. zur Überprüfung der Ausgabewerte, sowie zur Implementierung einer DMN Task erstellt worden. 

![alt text](https://github.com/MCikus/CamundaBPM-DMN-Implementation/blob/master/pictures/BPMN.png?raw=true "BPMN Prozess")

### Formular ausfüllen

Es ist wichtig das die Eingabewerte die **selbe ID und den selben Datentyp** tragen wie auch die Eingabewerte der DMN Tabellen. 

### Entscheidung finden

In der Decision Task wird unter dem Tab `General - Details` folgende Konfiguration vorgenommen.

1 Implementation -> DMN

2 Decision Ref -> InformationslisteID 
* gewünschte Ausgabe einer Decision des DRD durch die literal Expression ID oder Tabellen ID

3 Binding -> latest

4 Result Variable -> infoList
* Ausgabevariable der literal Expression, welche innerhalb der BPMN Engine verarbeitet werden soll (daher ein Array)

5 Map Decision Result -> singleEntry

### Kalkulation prüfen

In der Decision Task wird unter dem Tab `Listeners` folgende Konfiguration vorgenommen.

1 Execution Listener auf das **+** klicken

2 Event Type auf **start** setzen

3 Listener Type auf **Script** setzen

4 Script Format **Javascript** eingeben

5 Script Type auf **Inline Script** setzen

6 Innerhalb des Textfeldes Script folgenden Code einsetzen

```javascript
execution.setVariable("stunden", infoList[0]);
execution.setVariable("berateranzahl", infoList[1]);
execution.setVariable("preis", infoList[2]);
execution.setVariable("gesamtPreis", infoList[3]);
```

**Erklärung:**
Hiermit werden die einzelnen Werte aus der Position des Arrays geholt und in die Formularfelder eingesetzt, welche die selbe ID tragen müssen.  

***

## Testkonzept

Im folgendem Testkonzept werden alle möglichen Outputs der Decision **Gesamt** tabellarisch aufgeführt.

| Test-Szenario |  |  |  |  |  |  |  |  |
|----------------|-------------|----------------------------------------|-----------|------------|-----------------------|-----------------------|---------------------|---------------------|
| Anzahl Berater | Komplexität | Deadline | Enfernung | Auslastung | Ausgabe Stundenanzahl | Ausgabe Berateranzahl | Ausgabe Stundensatz | Ausgabe Gesamtpreis |
| integer | string | date | integer | boolean | double | double | double | double |
| >= 1 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 1 | 47,00 | 1410 |
| >= 2 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 2 | 49,00 | 2940 |
| >= 3 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 3 | 51,00 | 4590 |
| >= 4 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 4 | 53,00 | 6360 |
| >= 5 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 5 | 55,00 | 8250 |
| >= 6 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 6 | 57,00 | 10260 |
| >= 7 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | false | 30 | 7 | 59,00 | 12390 |
| >= 1 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 1 | 49,00 | 490 |
| >= 2 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 2 | 51,00 | 1020 |
| >= 3 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 3 | 53,00 | 1590 |
| >= 4 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 4 | 55,00 | 2200 |
| >= 5 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 5 | 57,00 | 2850 |
| >= 6 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 6 | 59,00 | 3540 |
| >= 7 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | false | 10 | 7 | 61,00 | 4270 |
| >= 1 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 1 | 50,00 | 250 |
| >= 2 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 2 | 52,00 | 520 |
| >= 3 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 3 | 54,00 | 810 |
| >= 4 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 4 | 56,00 | 1120 |
| >= 5 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 5 | 58,00 | 1450 |
| >= 6 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 6 | 60,00 | 1800 |
| >= 7 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | false | 5 | 7 | 62,00 | 2170 |
| >= 1 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 1 | 48,00 | 1440 |
| >= 2 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 2 | 50,00 | 3000 |
| >= 3 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 3 | 52,00 | 4680 |
| >= 4 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 4 | 54,00 | 6480 |
| >= 5 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 5 | 56,00 | 8400 |
| >= 6 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 6 | 58,00 | 10440 |
| >= 7 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | false | 30 | 7 | 60,00 | 12600 |
| >= 1 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 1 | 49,00 | 490 |
| >= 2 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 2 | 51,00 | 1020 |
| >= 3 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 3 | 53,00 | 1590 |
| >= 4 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 4 | 55,00 | 2200 |
| >= 5 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 5 | 57,00 | 2850 |
| >= 6 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 6 | 59,00 | 3540 |
| >= 7 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | false | 10 | 7 | 61,00 | 4270 |
| >= 1 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 1 | 50,00 | 250 |
| >= 2 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 2 | 52,00 | 520 |
| >= 3 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 3 | 54,00 | 810 |
| >= 4 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 4 | 56,00 | 1120 |
| >= 5 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 5 | 58,00 | 1450 |
| >= 6 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 6 | 60,00 | 1800 |
| >= 7 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | false | 5 | 7 | 62,00 | 2170 |
| >= 1 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 1 | 45,00 | 1350 |
| >= 2 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 2 | 47,00 | 2820 |
| >= 3 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 3 | 49,00 | 4410 |
| >= 4 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 4 | 51,00 | 6120 |
| >= 5 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 5 | 53,00 | 7950 |
| >= 6 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 6 | 55,00 | 9900 |
| >= 7 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | false | 30 | 7 | 57,00 | 11970 |
| >= 1 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 1 | 49,00 | 490 |
| >= 2 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 2 | 51,00 | 1020 |
| >= 3 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 3 | 53,00 | 1590 |
| >= 4 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 4 | 55,00 | 2200 |
| >= 5 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 5 | 57,00 | 2850 |
| >= 6 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 6 | 59,00 | 3540 |
| >= 7 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | false | 10 | 7 | 61,00 | 4270 |
| >= 1 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 1 | 50,00 | 250 |
| >= 2 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 2 | 52,00 | 520 |
| >= 3 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 3 | 54,00 | 810 |
| >= 4 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 4 | 56,00 | 1120 |
| >= 5 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 5 | 58,00 | 1450 |
| >= 6 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 6 | 60,00 | 1800 |
| >= 7 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | false | 5 | 7 | 62,00 | 2170 |
| >= 1 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 1 | 46,00 | 1380 |
| >= 2 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 2 | 48,00 | 2880 |
| >= 3 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 3 | 50,00 | 4500 |
| >= 4 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 4 | 52,00 | 6240 |
| >= 5 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 5 | 54,00 | 8100 |
| >= 6 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 6 | 56,00 | 10080 |
| >= 7 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | false | 30 | 7 | 58,00 | 12180 |
| >= 1 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 1 | 49,00 | 490 |
| >= 2 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 2 | 51,00 | 1020 |
| >= 3 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 3 | 53,00 | 1590 |
| >= 4 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 4 | 55,00 | 2200 |
| >= 5 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 5 | 57,00 | 2850 |
| >= 6 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 6 | 59,00 | 3540 |
| >= 7 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | false | 10 | 7 | 61,00 | 4270 |
| >= 1 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 1 | 50,00 | 250 |
| >= 2 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 2 | 52,00 | 520 |
| >= 3 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 3 | 54,00 | 810 |
| >= 4 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 4 | 56,00 | 1120 |
| >= 5 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 5 | 58,00 | 1450 |
| >= 6 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 6 | 60,00 | 1800 |
| >= 7 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | false | 5 | 7 | 62,00 | 2170 |
| >= 1 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 1 | 80,00 | 2400 |
| >= 2 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 2 | 80,00 | 4800 |
| >= 3 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 3 | 80,00 | 7200 |
| >= 4 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 4 | 80,00 | 9600 |
| >= 5 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 5 | 80,00 | 12000 |
| >= 6 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 6 | 80,00 | 14400 |
| >= 7 | hoch | < date and time("2019-08-04T00:00:00") | < 100 | true | 30 | 7 | 80,00 | 16800 |
| >= 1 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 1 | 80,00 | 800 |
| >= 2 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 2 | 80,00 | 1600 |
| >= 3 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 3 | 80,00 | 2400 |
| >= 4 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 4 | 80,00 | 3200 |
| >= 5 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 5 | 80,00 | 4000 |
| >= 6 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 6 | 80,00 | 4800 |
| >= 7 | mittel | < date and time("2019-08-04T00:00:00") | < 100 | true | 10 | 7 | 80,00 | 5600 |
| >= 1 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 1 | 80,00 | 400 |
| >= 2 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 2 | 80,00 | 800 |
| >= 3 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 3 | 80,00 | 1200 |
| >= 4 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 4 | 80,00 | 1600 |
| >= 5 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 5 | 80,00 | 2000 |
| >= 6 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 6 | 80,00 | 2400 |
| >= 7 | niedrig | < date and time("2019-08-04T00:00:00") | < 100 | true | 5 | 7 | 80,00 | 2800 |
| >= 1 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 1 | 80,00 | 2400 |
| >= 2 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 2 | 80,00 | 4800 |
| >= 3 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 3 | 80,00 | 7200 |
| >= 4 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 4 | 80,00 | 9600 |
| >= 5 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 5 | 80,00 | 12000 |
| >= 6 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 6 | 80,00 | 14400 |
| >= 7 | hoch | > date and time("2019-08-03T00:00:00") | < 100 | true | 30 | 7 | 80,00 | 16800 |
| >= 1 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 1 | 80,00 | 800 |
| >= 2 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 2 | 80,00 | 1600 |
| >= 3 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 3 | 80,00 | 2400 |
| >= 4 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 4 | 80,00 | 3200 |
| >= 5 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 5 | 80,00 | 4000 |
| >= 6 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 6 | 80,00 | 4800 |
| >= 7 | mittel | > date and time("2019-08-03T00:00:00") | < 100 | true | 10 | 7 | 80,00 | 5600 |
| >= 1 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 1 | 80,00 | 400 |
| >= 2 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 2 | 80,00 | 800 |
| >= 3 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 3 | 80,00 | 1200 |
| >= 4 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 4 | 80,00 | 1600 |
| >= 5 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 5 | 80,00 | 2000 |
| >= 6 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 6 | 80,00 | 2400 |
| >= 7 | niedrig | > date and time("2019-08-03T00:00:00") | < 100 | true | 5 | 7 | 80,00 | 2800 |
| >= 1 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 1 | 80,00 | 2400 |
| >= 2 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 2 | 80,00 | 4800 |
| >= 3 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 3 | 80,00 | 7200 |
| >= 4 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 4 | 80,00 | 9600 |
| >= 5 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 5 | 80,00 | 12000 |
| >= 6 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 6 | 80,00 | 14400 |
| >= 7 | hoch | < date and time("2019-08-04T00:00:00") | >= 100 | true | 30 | 7 | 80,00 | 16800 |
| >= 1 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 1 | 80,00 | 800 |
| >= 2 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 2 | 80,00 | 1600 |
| >= 3 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 3 | 80,00 | 2400 |
| >= 4 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 4 | 80,00 | 3200 |
| >= 5 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 5 | 80,00 | 4000 |
| >= 6 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 6 | 80,00 | 4800 |
| >= 7 | mittel | < date and time("2019-08-04T00:00:00") | >= 100 | true | 10 | 7 | 80,00 | 5600 |
| >= 1 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 1 | 80,00 | 400 |
| >= 2 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 2 | 80,00 | 800 |
| >= 3 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 3 | 80,00 | 1200 |
| >= 4 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 4 | 80,00 | 1600 |
| >= 5 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 5 | 80,00 | 2000 |
| >= 6 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 6 | 80,00 | 2400 |
| >= 7 | niedrig | < date and time("2019-08-04T00:00:00") | >= 100 | true | 5 | 7 | 80,00 | 2800 |
| >= 1 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 1 | 80,00 | 2400 |
| >= 2 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 2 | 80,00 | 4800 |
| >= 3 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 3 | 80,00 | 7200 |
| >= 4 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 4 | 80,00 | 9600 |
| >= 5 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 5 | 80,00 | 12000 |
| >= 6 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 6 | 80,00 | 14400 |
| >= 7 | hoch | > date and time("2019-08-03T00:00:00") | >= 100 | true | 30 | 7 | 80,00 | 16800 |
| >= 1 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 1 | 80,00 | 800 |
| >= 2 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 2 | 80,00 | 1600 |
| >= 3 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 3 | 80,00 | 2400 |
| >= 4 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 4 | 80,00 | 3200 |
| >= 5 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 5 | 80,00 | 4000 |
| >= 6 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 6 | 80,00 | 4800 |
| >= 7 | mittel | > date and time("2019-08-03T00:00:00") | >= 100 | true | 10 | 7 | 80,00 | 5600 |
| >= 1 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 1 | 80,00 | 400 |
| >= 2 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 2 | 80,00 | 800 |
| >= 3 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 3 | 80,00 | 1200 |
| >= 4 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 4 | 80,00 | 1600 |
| >= 5 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 5 | 80,00 | 2000 |
| >= 6 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 6 | 80,00 | 2400 |
| >= 7 | niedrig | > date and time("2019-08-03T00:00:00") | >= 100 | true | 5 | 7 | 80,00 | 2800 |
