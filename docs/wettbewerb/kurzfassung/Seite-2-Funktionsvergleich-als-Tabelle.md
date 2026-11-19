**twx Solutions GmbH**

Entscheidungsgrundlage für die Commerzbank

BAW-Regelbetrieb

# Funktionsvergleich

Funktionen für Entwicklung und IT-Betrieb

| Anforderung                                  | twx Analyzer · twx Solutions GmbH                                         | Fuchs · IT-Consulting · v2.0.0           | Dosvak · 1.3.1 · WAR                           | Salient Process · Analyzer & Migrator       |
|----------------------------------------------|---------------------------------------------------------------------------|------------------------------------------|------------------------------------------------|---------------------------------------------|
| Helm-Installation auf OpenShift              | **Vorhanden** · Helm-Chart mitgeliefert                                   | **Fehlt**                                | **Fehlt** · WAR-Deployment                     | **Nicht belegt**                            |
| Direkte AD-/LDAP-Anmeldung                   | **Vorhanden** · LDAP-Anbindung                                            | **Fehlt** · Ohne Login                   | **Fehlt** · Firmenlogin über separaten Dienst  | **Nicht belegt**                            |
| App-Zugriff nach Gruppen in Active Directory | **Vorhanden** · Snapshot-Liste und Analyse                                | **Fehlt** · Keine App-Zuordnung          | **Teilweise** · Rechte je Workspace            | **Nicht belegt**                            |
| Regelflexibilität durch Plugins              | **Vorhanden** · Separat installierbare Plugin-JARs                        | **Teilweise** · Java-Regeln, neuer Build | **Teilweise** · Eigene Regeln per Java-API     | **Nicht belegt**                            |
| Projekt- und teamspezifische Regeln          | **Vorhanden** · Eigene Plugin-Pakete                                      | **Teilweise** · Regelauswahl je Snapshot | **Vorhanden** · Regelrichtlinien je Team       | **Nicht belegt**                            |
| Regelzuordnung je App und Toolkit            | **Vorhanden** · Dauerhafte Zuordnung                                      | **Teilweise** · Global oder je Snapshot  | **Teilweise** · Je Arbeitsbereich              | **Nicht belegt**                            |
| XML-Diff mit XPath-Filtern                   | **Vorhanden** · XPath-Filter und Verlauf                                  | **Teilweise** · Feste Kontextzeilen      | **Fehlt** · Artefakt- und Befundvergleich      | **Nicht belegt** · Export für externes Diff |
| Volltextsuche über mehrere Snapshots         | **Vorhanden** · Indizierte Volltextsuche + RegEx-Suche für Feldreferenzen | **Vorhanden** · Volltext und Regex       | **Teilweise** · Im ausgewählten TWX            | **Nicht belegt**                            |
| Toolkit-Snapshots direkt in BAW bereinigen   | **Vorhanden** · Archivieren und löschen                                   | **Fehlt**                                | **Fehlt**                                      | **Nicht belegt**                            |
| BPMN aus unterstützten BAW-Prozessen         | **Vorhanden** · BPMN-Export und Bearbeitung                               | **Fehlt** · TWX-Diagrammansicht          | **Fehlt** · TWX-Diagrammansicht                | **Nicht belegt**                            |
| Snapshot-Analyse in CI/CD                    | **Vorhanden** · Befundzahlen per REST. Error, Warning und Bad Style       | **Vorhanden** · REST-Quality-Gate        | **Vorhanden** · REST-Quality-Gate              | **Nicht belegt**                            |
| Kundenlogo und Farbschema                    | **Vorhanden** · CoBa-Look-and-Feel                                        | **Fehlt** · Festes Branding              | **Teilweise** · Anwendungstitel konfigurierbar | **Nicht belegt**                            |

**Bewertung.** „Teilweise“ heißt, dass ein Teil der Funktion vorhanden ist. Für „Nicht belegt“ fehlt ein Nachweis.
