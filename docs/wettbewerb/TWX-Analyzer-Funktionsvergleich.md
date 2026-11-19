# Funktionsvergleich des TWX Analyzers mit drei Alternativen

Der TWX Analyzer von twx Solutions liegt im dokumentierten Funktionsreifegrad vorn, wenn der laufende Betrieb von IBM Business Automation Workflow (BAW) im Vordergrund steht. Er verbindet wiederkehrende Snapshot-Analysen mit eigenen Prüf-Plugins, CI/CD-Anbindung und einem dokumentierten Betriebsmodell auf OpenShift.

## 1. Reifegrad und Sicherheit der Bewertung

Der Vergleich stützt sich auf die beiden Funktionsübersichten. Für twx Solutions liegen Referenzen auf die geprüften lokalen Stände von Web-App, Backend und Helm vor. Die Wettbewerbsübersicht vom 10. November 2026 erfasst Dosvak 1.3.1, Fuchs v2.0.0 und bei Salient auch historische Produktunterlagen. Die technischen Betriebsdetails sind je nach Produkt unterschiedlich ausführlich dokumentiert. [Funktionsübersicht twx Solutions](../produkt/Funktionen-Gesamt%C3%BCbersicht.md) · [Wettbewerbsübersicht](Drei-Konkurenten-Funktionen.md)

Der Reifegrad bewertet den dokumentierten Funktionsumfang und die Tiefe der Abläufe in fünf gleich gewichteten Bereichen. Je Bereich sind bis zu 20 Punkte möglich. Als Orientierung stehen 0 Punkte für fehlende Nachweise, 10 für Grundfunktionen, 15 für einen durchgängigen Ablauf und 20 für eine besonders umfassende Abdeckung im jeweiligen Bereich.

Der Confidence-Score ist eine separate Einschätzung der Beleglage auf einer Skala von 0 bis 100. Für twx Solutions und Dosvak sind auch Integrations- und Betriebsabläufe im Detail beschrieben. Fuchs ist versionsbezogen dokumentiert, mit weniger Angaben zum betrachteten Betriebsmodell. Bei Salient bleibt der heutige Umfang der Self-Service-Anwendung offen.

| Bewertung | TWX Analyzer (twx Solutions) | TWX Code Analyzer (Dosvak) | TWX Analyzer v2 (Fuchs IT-Consulting) | TWX Analyzer & Migrator (Salient Process) |
| --- | ---: | ---: | ---: | ---: |
| Codeprüfung und Befundbearbeitung | 17 | 19 | 18 | 13 |
| Bestandserkundung, Suche und Vergleich | 18 | 17 | 19 | 14 |
| Integration, Automatisierung und Erweiterbarkeit | 18 | 17 | 15 | 10 |
| Betrieb und Zugriffsverwaltung | 18 | 17 | 14 | 11 |
| Technische Weiterverarbeitung und Bestandspflege | 18 | 13 | 13 | 19 |
| **Reifegrad von 100** | **89** | **83** | **79** | **67** |
| Confidence-Score von 100 | 85 | 85 | 80 | 65 |

## 2. Kernfunktionen und direkter Vergleich

- **twx Solutions.** Regelbasierte Qualitätsprüfung für BAW-Snapshots im laufenden Betrieb. Schwerpunkte sind kundenspezifische Prüfungen, die Einbindung in Release-Pipelines und die Verwaltung angebundener BAW-Bestände. [Funktionsübersicht, Zeilen 7–57 und 59–78](../produkt/Funktionen-Gesamt%C3%BCbersicht.md)
- **Dosvak.** Statische Codeanalyse mit Belegen an der Fundstelle, Berichten und einem Team-Workflow für Review und Freigabe. Dafür gibt es eine lokale Browseroberfläche, eine Serveredition und eine BAW-Prozessanwendung. [Wettbewerbsübersicht, Zeilen 33–85](Drei-Konkurenten-Funktionen.md#twx-code-analyzer-von-dosvak)
- **Fuchs IT-Consulting.** Analyse von App- und Toolkit-Beständen über mehrere Stände hinweg. Schwerpunkte sind globale Suche, Objektvergleich und ein Toolkit-Inventar mit Nutzungsnachweisen. [Wettbewerbsübersicht, Zeilen 87–112](Drei-Konkurenten-Funktionen.md#twx-analyzer-v2-von-fuchs-it-consulting)
- **Salient Process.** Bewertung und Vorbereitung von IBM-Cloud-Migrationen. Der Analyzer ermittelt Abhängigkeiten und Migrationshindernisse. Der Migrator automatisiert Änderungen an IBM-Artefakten. [Wettbewerbsübersicht, Zeilen 5–31](Drei-Konkurenten-Funktionen.md)

| Bereich | twx Solutions | Dosvak | Fuchs IT-Consulting | Salient Process |
| --- | --- | --- | --- | --- |
| Import und Analysebestand | TWX-Upload, BAW-Abruf, Autoload des neuesten Track-Snapshots, Reanalyse und ISF-Archivprüfung per REST | TWX-Upload, in der Serveredition IBM-Direktimport für Snapshot oder Track-Tip, Berichtshistorie | Mehrfach-Upload, lesender BAW-Import, Projekt- und Snapshot-Historie mit erneuter Analyse | TWX-Upload in historischer Web-Oberfläche, heutiger Self-Service-Zugang offen |
| Prüfregeln und Befunde | Plugin-Regeln je App und Toolkit, eigene Plugin-JARs, Befunde je Service, dauerhafte Ausnahmen | 156 Regeln, regelbezogene Gewichte, Befundbelege, in der Serveredition versionierte Richtlinien und begründete Befundakzeptanz | 156 Regeln auf Basis des Dosvak-Regelkerns, Einstellungen je Snapshot, Befundstatus und Kommentare | Prüfungen auf Cloud-Migrationshindernisse und ungenutzte Artefakte |
| Freigabe und Berichte | REST liefert Befundzahlen für Freigaberegeln in CI/CD, CSV je Befundtyp | Integrierte Quality Gates in der Serveredition, Dashboard, Executive-PDF, CSV-, HTML- und PDF-Exporte | Integriertes Quality Gate, Health-Verlauf, JSON-, CSV- und HTML-Exporte | Aufwandsschätzung, Migrationsberichte und CSV für viele Berichte |
| Suche und Exploration | Volltext über indexierte Stände und unterstützte Textressourcen, Service-Repository, Feldreferenzen und Übersetzungen | Text- und Regex-Suche im gewählten TWX, Objektbrowser, Diagramme mit Befundsprung | Globale Volltext- und Regex-Suche, bidirektionale Referenzen und Variablenverwendungen | Artefaktinventar, Abhängigkeitsvisualisierung und projektübergreifende Nutzungsanalyse |
| Versionsvergleich | XML-Vergleich mit Verlauf und XPath-Filtern bei eingerichtetem Remote-Abruf und Datei-Aufbewahrung | Vergleich von Befunden, Artefakten, Toolkit-Versionen und Health | Befund- und Objektvergleich, XML-Diff für gespeicherte Textinhalte, teilbarer Direktlink | Berichte für Toolkit-Snapshots und Tracks, Dateien für ein separates Detail-Diff-Werkzeug |
| Toolkit-Abhängigkeiten | Filterbarer Toolkit-Tree für direkte und verschachtelte Abhängigkeiten mit Toolkit-, Snapshot- und Track-Namen | Aufklappbarer Nutzungsbericht mit referenzierenden Artefakten und Verweisen zwischen Toolkits | Abhängigkeitsansicht je Snapshot und Bäume im globalen Toolkit-Inventar | Abhängigkeitsbäume und Sunburst-Diagramm für die Migrationsplanung |
| Toolkit-Versionskonflikte | `Toolkit Conflict` (`79`, `Error`) meldet mehrere Snapshot-Stände desselben Toolkit-Projekts und nennt je Stand die importierenden Snapshots | `TCA-APP-002` nennt das Toolkit und die betroffenen Snapshot-Namen | `R-APP-002` und separate Konfliktansicht mit Verwendungsstellen | Versionsübersichten für die Migration. Eine gleichartige Konfliktregel ist nicht belegt |
| Toolkit-Pflege | Verwaiste Toolkit-Snapshots direkt in BAW ermitteln, archivieren und löschen | Gespeicherte Analysen verwalten | Globales Inventar mit Erkennung identischer ZIP-Inhalte und lokalen Löschfunktionen | Migrationsreihenfolge aus Abhängigkeiten, Bereinigung ungenutzter Artefakte und Abhängigkeiten im Migrator |
| Migration und Weiterverwendung | Unterstützte BAW-Prozessdiagramme als BPMN mit Layout erzeugen, bearbeiten und exportieren | Interaktive Prozess- und Servicediagramme zur Analyse | Interaktive Prozessdiagramme bei vorhandenen Layoutdaten | Artefakttypen modernisieren, Toolkit-Versionen vereinheitlichen und UI-Controls per Transformer umstellen |
| Betrieb und Zugriff | Helm auf OpenShift, LDAP bzw. Active Directory, LDAP-Gruppen für App-Zugriff im Mandantenmodus | Serveredition mit optionalem SSO, Team-Arbeitsbereichen, Audit-Protokoll und Metriken | Projekt-Dashboard und HTTPS-Anbindung an BAW mit optionalem eigenem CA-Bundle | Historische Online-Oberfläche und dokumentierte Transformer-Abläufe |

Die Funktionsverfügbarkeit hängt bei twx Solutions und bei Dosvaks Serveredition von Einrichtung und Berechtigungen ab. [Funktionsübersicht twx Solutions](../produkt/Funktionen-Gesamt%C3%BCbersicht.md) · [Wettbewerbsübersicht](Drei-Konkurenten-Funktionen.md)

## 3. Stärken und Schwächen je Anwendung

| Anwendung | Stärken | Grenzen im betrachteten Einsatz |
| --- | --- | --- |
| **twx Solutions** | Eigene Prüf-Plugins und Zuordnung von Prüfregeln je App und Toolkit. CI/CD-Analyse, LDAP-gestützter App-Zugriff und OpenShift-Installation sind dokumentiert. Das Produkt ermöglicht auch die Bereinigung in BAW und die Weiterverarbeitung von BPMN-Dateien. | Der XML-Vergleich setzt Remote-Abruf und Datei-Aufbewahrung voraus. Die Bearbeitung im BPMN-Viewer setzt eine Freischaltung voraus. Der dokumentierte Befundexport ist auf CSV je Befundtyp ausgerichtet. Die Entscheidung über eine Freigabe liegt beim beschriebenen CI/CD-Ablauf in der Pipeline. |
| **Dosvak** | Detaillierte Befundbelege, Diagnosehinweise und direkte Sprünge zur Fundstelle. Die Serveredition bietet versionierte Regeln, differenzierte Quality Gates, Audit-Protokoll, Benachrichtigungen und Executive-PDF. | Die beschriebenen Team- und Freigabefunktionen setzen die Serveredition und passende Rechte voraus. Für das separate CP4BA-Paket sind Import und Ansicht im Designer dokumentiert. Die Übersicht weist weder eine Erweiterung um eigene Analyse-Plugins noch eine direkte BAW-Snapshot-Bereinigung nach. |
| **Fuchs IT-Consulting** | Starke Bestandserkundung mit Volltext und Regex, XML-Diff und Variablenverwendungen. Das globale Toolkit-Inventar zeigt Nutzung und veraltete Abhängigkeiten. Die Snapshot-Ansicht zeigt Versionskonflikte und Zyklen. | Der BAW-Import ist ausdrücklich lesend. Löschungen im Toolkit-Inventar betreffen den lokalen Analysebestand. Der XML-Detailvergleich greift auf gespeicherte Textinhalte zurück. Eigene Analyse-Plugins und das vorgesehene LDAP-/OpenShift-Betriebsmodell sind in der Übersicht offen. |
| **Salient Process** | Spezialisierte Migrationsbewertung mit Aufwandsabschätzung, Abhängigkeitsreihenfolge, Cloud-Hindernissen und dokumentierten Artefakt-Transformationen. | Der heutige Zugang zur historischen Self-Service-Anwendung und der aktuell selbst bedienbare Transformer-Umfang bleiben offen. Der dokumentierte Detailvergleich benötigt ein separates Werkzeug. Ein vergleichbarer, kontinuierlicher CI/CD-Prüfprozess ist aus der Übersicht nicht ersichtlich. |

[Funktionsübersicht twx Solutions, Zeilen 21, 32–50 und 53–78](../produkt/Funktionen-Gesamt%C3%BCbersicht.md) · [Wettbewerbsübersicht](Drei-Konkurenten-Funktionen.md)

## 4. Argumente für den TWX Analyzer

> Mit dem TWX Analyzer prüfen wir BAW-Snapshots wiederkehrend nach unseren eigenen Regeln. Wir können Prüfregeln je Prozessanwendung und Toolkit zuordnen und Befundzahlen über REST in unsere Release-Pipeline übernehmen. Auf OpenShift lässt sich der Zugriff auf die zugewiesenen Prozessanwendungen über LDAP-Gruppen steuern. So können Entwicklung und Betrieb dieselben Prüfungen für die Befundbearbeitung und die Release-Entscheidung nutzen.

- **Gegenüber Dosvak** liegt der Vorteil bei eigenen ausführbaren Prüf-Plugins und der dokumentierten Zuordnung von Prüfregeln zu Apps und Toolkits im vorgesehenen Betriebsmodell. Wenn ein integriertes Quality Gate und versionierte zentrale Richtlinien den Ausschlag geben, hat Dosvak in der Serveredition die stärkere Abdeckung. [twx Solutions, Zeilen 48–50, 54, 57, 59 und 71](../produkt/Funktionen-Gesamt%C3%BCbersicht.md) · [Dosvak, Zeilen 59–75](Drei-Konkurenten-Funktionen.md#zusätzliche-funktionen-der-serveredition-war)
- **Gegenüber Fuchs** ist die Pflege des BAW-Quellsystems ein konkreter Unterschied, sofern sie zum Auftrag gehört. Berechtigte Personen können verwaiste Toolkit-Snapshots im angebundenen BAW archivieren und anschließend löschen. Fuchs dokumentiert einen lesenden BAW-Import und die Bereinigung seines eigenen Inventars. [twx Solutions, Zeilen 34–35](../produkt/Funktionen-Gesamt%C3%BCbersicht.md) · [Fuchs, Zeilen 94 und 108–109](Drei-Konkurenten-Funktionen.md#twx-analyzer-v2-von-fuchs-it-consulting)
- **Gegenüber Salient** spricht der wiederkehrende Prüfablauf mit Reanalyse, Release-Pipeline und App-Zugriff über LDAP-Gruppen für twx Solutions. Für IBM-Cloud-Migrationen mit Artefakt-Modernisierung liefert Salient mehr dokumentierte Spezialfunktionen. Wenn bearbeitbare BPMN-Dateien für die Weiterverwendung erforderlich sind, kann twx Solutions unterstützte BAW-Prozessdiagramme in diesem Format ausgeben. [twx Solutions, Zeilen 8–12, 36–42, 54 und 57](../produkt/Funktionen-Gesamt%C3%BCbersicht.md) · [Salient, Zeilen 13–29](Drei-Konkurenten-Funktionen.md)
- **Toolkit-Abhängigkeiten vor der Freigabe prüfen.** Im Toolkit-Tree sehen Teams, über welche Toolkits eine Prozessanwendung weitere Toolkits einbindet. Der Fehlerbefund **Toolkit Conflict** nennt die parallel eingebundenen Stände desselben Toolkits und die Snapshots, die sie jeweils importieren. So lassen sich die Stellen für eine Versionsangleichung finden. [Toolkit-Tree](../produkt/Funktionen-Gesamt%C3%BCbersicht.md#L28) · [Befunddetails](../../../TWX-Analyzer-AllComponents-SpringBoot2/src/main/java/ssee/twxanalyzerbackend/core/TwxInterpreterImpl.java#L243)

## 5. Kurzvermerk für die Beschaffungsdokumentation

*Textvorschlag auf Basis eines aus den Übersichten abgeleiteten Anforderungsprofils. Vor der Übernahme ist abzugleichen, welche Kriterien für den konkreten Einsatz verbindlich sind.*

Für den vorgesehenen IBM-BAW-Regelbetrieb wurden der TWX Analyzer von twx Solutions, der TWX Code Analyzer von Dosvak, der TWX Analyzer v2 von Fuchs IT-Consulting und der TWX Analyzer & Migrator von Salient Process anhand der vorliegenden Funktionsübersichten verglichen. Maßgeblich sind wiederholbare Snapshot-Analysen, eigene Prüflogik mit Zuordnung je Prozessanwendung und Toolkit sowie maschinell auswertbare Befundzahlen für CI/CD-Freigaben. Die Lösung soll sich per Helm auf OpenShift installieren lassen und den Zugriff auf zugewiesene Prozessanwendungen über LDAP-Gruppen steuern.

- **Dosvak** bietet IBM-Direktimport, Team-Arbeitsbereiche und Quality Gates. Die Unterlagen belegen die Erweiterung um eigene Prüflogik je App und Toolkit und den geforderten Helm-/OpenShift-Betrieb mit LDAP-gestütztem App-Zugriff nicht ausreichend. [Dosvak, Zeilen 57–75](Drei-Konkurenten-Funktionen.md#zusätzliche-funktionen-der-serveredition-war)
- **Fuchs IT-Consulting** deckt den lesenden BAW-Import, die Bestandsanalyse und den Versionsvergleich ab. Für kundenspezifisch erweiterbare Prüflogik je App und Toolkit und das genannte Betriebs- und Zugriffsmodell fehlt ein ausreichender Nachweis. [Fuchs, Zeilen 87–112](Drei-Konkurenten-Funktionen.md#twx-analyzer-v2-von-fuchs-it-consulting)
- **Salient Process** deckt vor allem die Bewertung und Umstellung von Anwendungen für IBM-Cloud-Migrationen ab. Die Unterlagen belegen den wiederkehrenden Snapshot-Prüfprozess mit eigenen Prüfregeln und Anbindung an CI/CD-Freigaben nicht ausreichend. [Salient, Zeilen 5–31](Drei-Konkurenten-Funktionen.md)

Für die fachliche Auswahl spricht, dass der TWX Analyzer von twx Solutions diese Anforderungen laut Funktionsübersicht bei entsprechender Einrichtung gemeinsam abdeckt. [Funktionsübersicht twx Solutions, Zeilen 7–15, 48–50, 53–59 und 71](../produkt/Funktionen-Gesamt%C3%BCbersicht.md)

## Anhang zur Bewertung

- **Codeprüfung und Befundbearbeitung.** Welche Fehler und Risiken die Prüfregeln erkennen und wie Teams die Befunde bearbeiten können. Dazu zählen genaue Fundstellen, Behebungshinweise und der Umgang mit akzeptierten oder ignorierten Befunden.
- **Bestandserkundung, Suche und Vergleich.** Wie sich Apps, Toolkits und Artefakte in den vorhandenen Snapshots untersuchen lassen. Bewertet werden etwa die Suche in Skripten oder XML, die Anzeige von Abhängigkeiten und der Vergleich von Änderungen zwischen zwei Ständen.
- **Integration, Automatisierung und Erweiterbarkeit.** Wie sich der Analyzer in bestehende Entwicklungs- und Freigabeabläufe einbinden und an eigene Prüfanforderungen anpassen lässt. Beispiele sind der direkte BAW-Abruf, die Übergabe von Befundzahlen an eine CI/CD-Pipeline und die Konfiguration oder Erweiterung der Prüfregeln.
- **Betrieb und Zugriffsverwaltung.** Wie sich das Produkt installieren, überwachen und für verschiedene Benutzergruppen betreiben lässt. Hier zählen unter anderem die dokumentierte Bereitstellung, die Anmeldung, Berechtigungen je App oder Arbeitsbereich und Betriebsmetriken.
- **Technische Weiterverarbeitung und Bestandspflege.** Was Teams mit den analysierten Artefakten und Toolkit-Ständen anschließend tun können. Dazu zählen der Export bearbeitbarer BPMN-Dateien, die Modernisierung von IBM-Artefakten und die Bereinigung ungenutzter Toolkit-Einträge im Inventar oder direkt in BAW, je nach Produkt.
- **Reifegrad von 100.** Die Summe der fünf Teilbewertungen mit je bis zu 20 Punkten. Beim TWX Analyzer von twx Solutions ergeben 17 + 18 + 18 + 18 + 18 Punkte den Reifegrad von 89.
- **Confidence-Score von 100.** Eine separate Einschätzung, wie gut die Quellen die jeweilige Bewertung stützen. Detailliert dokumentierte Abläufe und eindeutige Versionsangaben erhöhen die Sicherheit. Historische Unterlagen und offene Fragen zur heutigen Verfügbarkeit verringern sie.
