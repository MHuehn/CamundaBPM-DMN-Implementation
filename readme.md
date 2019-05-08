# CamundaBPM DMN Implementation

In dieser Dokumentation wird eine DMN Task über die Camunda BPM Plattform mittels der DMN Implementation ausgeführt. Dabei werden auf die einzelnen Schritte eingegangen die für die Implementierung relevant sind.

Die Dokumentation setzt voraus, dass die Camunda BPM Plattform und seine zusätzlichen Pakete installiert sind. Auch wird nicht näher erläutert wie andere Tasks (User-Task) innerhalb des BPMN-Modells zu konfigurieren ist. 

Sollten hier wissenslücken aufkommen, wird die Camunda BPM Dokumentation empfohlen: <https://docs.camunda.org/manual/7.10/>

Für eine breite exemplarische Implementierung wird das folgene Projekt empfohlen: https://github.com/strumswell/softwareauswahl-kgwebservice

Dabei wurde ein DRD als Basis für fünf wesentliche Decisions erstellt. Die **Informationen** Decision erzeugt ein notwendiges Array um die Ausgabewerte der einzelnen Tabellen für die Übergabe innerhalb der Camunda BPM Engine zu ermöglichen. Im DRD sind neben der Decisions auch fünf Input Data Shapes sowie jeweils eine Business Knowledge Rule und eine Knowledge Source. Diese weiteren Shapes tragen keine technischen Konfigurationen und tragen dem besseren Verständnis bei, welcher logische Kontext hinter den einzelnen DMN Tabellen steht.
