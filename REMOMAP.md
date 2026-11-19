# Repository-Map

Diese Map beschreibt den lokalen Dateibestand nach der Neuordnung am 19.11.2026. Sie ordnet vorhandene Unterlagen ein; die fachlichen Aussagen der Dokumente wurden dabei nicht neu validiert.

## Einstieg

- [README.md](README.md): Kurzüberblick und Einstieg in die Ordner.
- [AGENTS.md](AGENTS.md): Arbeitsregeln für dieses Repository.
- [REMOMAP.md](REMOMAP.md): vollständiger Dateiüberblick.
- [.gitignore](.gitignore): ignoriert .idea/.

## Ordnerstruktur

```text
TWX-Analyzer-Data-Files/
├── AGENTS.md
├── README.md
├── REMOMAP.md
├── docs/
│   ├── produkt/
│   ├── technik/
│   ├── benutzer/
│   ├── wettbewerb/
│   │   └── kurzfassung/
│   ├── marketing/
│   └── recht/
├── assets/
│   ├── screenshots/fuchs/
│   └── pdfs/
│       ├── vergleich/
│       └── referenzen/
└── beispiele/twx/
```

## Dateiverzeichnis

### docs/produkt

Produktfunktionen und Release-Hinweise.

| Datei | Inhalt |
| --- | --- |
| [ChangeLog und Release Notes.md](docs/produkt/ChangeLog%20und%20Release%20Notes.md) | Release-Hinweise bis Version 2.3.8; bestehende CP1252-Kodierung beibehalten. |
| [Funktionen-Gesamtübersicht.md](docs/produkt/Funktionen-Gesamt%C3%BCbersicht.md) | Funktionskatalog mit Verweisen auf die benachbarten Quellcode-Repositories. Die Übersicht verbindet die beschriebenen Funktionen mit Fundstellen in Backend, Frontend und Helm-Chart. |

### docs/technik

Technische Dokumentation.

| Datei | Inhalt |
| --- | --- |
| [Plugins-deep-dive(2).md](docs/technik/Plugins-deep-dive%282%29.md) | Plugin-Lebenszyklus, Objekttypen, FindingTypes und Konfiguration. Ergänzt die Ablaufbeschreibung um Empfehlungen zur flexibleren Regelkonfiguration, ein Plugin-JAR-Inventar und ein Glossar. |
| [Twx-File-and-PackageData-Layout.md](docs/technik/Twx-File-and-PackageData-Layout.md) | TWX-Archivstruktur, package.xml und Zuordnung zu Laufzeitklassen. Beschreibt außerdem Abhängigkeiten, eingebettete Toolkits und spezielle Metadatenobjekte. |

### docs/benutzer

Benutzerdokumentation und Originalexport.

| Datei | Inhalt |
| --- | --- |
| [front-end-user-documentation.zip](docs/benutzer/front-end-user-documentation.zip) | Originalarchiv der Frontend-Dokumentation mit 110 Einträgen unter docs/. Bewahrt die einzelnen Handbuchseiten samt Unterordnern, Beispielen und ergänzenden Ressourcen. |
| [frontend_combined_documentation.md](docs/benutzer/frontend_combined_documentation.md) | Zusammengeführter Dokumentationsexport zu Installation, Bedienung und Plugins. Enthält die Handbuchtexte in einer Datei; Verweise auf ursprüngliche Einzeldateien sind darin teilweise nicht auflösbar. |

### docs/wettbewerb

Wettbewerbsanalysen und Funktionsvergleiche.

| Datei | Inhalt |
| --- | --- |
| [Drei-Konkurenten-Funktionen.md](docs/wettbewerb/Drei-Konkurenten-Funktionen.md) | Zusammenstellung zu Salient Process, Dosvak und Fuchs IT-Consulting. Unterscheidet unter anderem Analyzer- und Migrationsfunktionen sowie lokale Browser-, Web- und Servereditionen. |
| [Drei-Konkurenten-Funktionen_erweitertFuchs.md](docs/wettbewerb/Drei-Konkurenten-Funktionen_erweitertFuchs.md) | Erweiterter Vergleich mit Einzelfunktionsabgleichen für Dosvak und Fuchs. Ordnet den Dosvak-Abgleich nach Bereichen wie Start, Analyze, Develop, Explore, Integration und Administration. |
| [TWX-Analyzer-Funktionsvergleich-Fuchs-Perspektive.md](docs/wettbewerb/TWX-Analyzer-Funktionsvergleich-Fuchs-Perspektive.md) | Vergleich aus Fuchs-Perspektive für Bestandserkundung und Qualitätsreview. Legt die Vergleichsbasis offen und führt Quellen zu den bewerteten Kriterien auf. |
| [TWX-Analyzer-Funktionsvergleich.md](docs/wettbewerb/TWX-Analyzer-Funktionsvergleich.md) | Vergleich mit drei Alternativen, Argumentation und Beschaffungsvermerk. Behandelt auch die Sicherheit der Bewertung sowie Stärken und Schwächen der einzelnen Anwendungen. |
| [TWX-Analyzer-Funktionsvergleich_V2.md](docs/wettbewerb/TWX-Analyzer-Funktionsvergleich_V2.md) | Erweiterte Vergleichsfassung einschließlich OpenShift, Active Directory und XML-Diff. Vertieft außerdem Code-Review und Qualitätsfreigaben bei Dosvak sowie die IBM-Cloud-Migration mit Salient Process. |

### docs/wettbewerb/kurzfassung

Vierseitige Kurzfassung des Vergleichs.

| Datei | Inhalt |
| --- | --- |
| [Seite-1-Die-wichtigsten-Tools-Funktionen.md](docs/wettbewerb/kurzfassung/Seite-1-Die-wichtigsten-Tools-Funktionen.md) | Kurzfassung der wichtigsten Vorteile. Bündelt sechs Argumente für den BAW-Regelbetrieb, darunter Helm-Installation, App-Zugriff, Plugins, XML-Diff, Toolkit-Bereinigung und BPMN-Bearbeitung. |
| [Seite-2-Funktionsvergleich-als-Tabelle.md](docs/wettbewerb/kurzfassung/Seite-2-Funktionsvergleich-als-Tabelle.md) | Tabellarischer Funktionsvergleich. Stellt Anforderungen aus Entwicklung und IT-Betrieb für vier Produkte gegenüber und unterscheidet vorhandene, teilweise vorhandene, fehlende und nicht belegte Funktionen. |
| [Seite-3-Weitere-Tools-und-Anbieter-Betriebsmodelle.md](docs/wettbewerb/kurzfassung/Seite-3-Weitere-Tools-und-Anbieter-Betriebsmodelle.md) | Ergänzungen zu Funktionen, Anbietern und Betriebsmodellen. Behandelt unter anderem Autoload, Prüfausnahmen, TODOs und Datensicherung sowie Quellcode, Speicherung und Betriebsformen der Alternativen. |
| [Seite-4-Negativmerkmale.md](docs/wettbewerb/kurzfassung/Seite-4-Negativmerkmale.md) | Zusammenstellung von Negativmerkmalen der Alternativen. Fasst die im Dokument bewerteten Betriebsanforderungen, Grenzen der Funktionsnachweise und Angaben zur Projekthistorie zusammen. |

### docs/marketing

Website- und Präsentationsinhalte.

| Datei | Inhalt |
| --- | --- |
| [InhaltWebSeite_Aktuell_13012026.md](docs/marketing/InhaltWebSeite_Aktuell_13012026.md) | Vorhandener Website-Text mit Datumsangabe im Dateinamen. Beschreibt Code-Analyse und Code-Governance, typische BPM-Projektprobleme und einen fünfteiligen Workflow von Start bis Customize. |
| [Präsentations_folien_Inhalt.md](docs/marketing/Pr%C3%A4sentations_folien_Inhalt.md) | Folieninhalte zu Produkt, Herausforderungen, Plugins und Lizenzmodellen. Sechs Folien führen von typischen Entwicklungsproblemen über die Plugin-Architektur zum Produktnutzen und zum Vergleich von Einzel- und Enterprise-Lizenz. |

### docs/recht

Rechtliche Dokumente.

| Datei | Inhalt |
| --- | --- |
| [AGBs.md](docs/recht/AGBs.md) | Allgemeine Geschäftsbedingungen für Softwareüberlassung und Dienstleistungen. Regelt unter anderem Vertragsschluss, Mitwirkungspflichten, Datensicherung und Vergütung. |

### assets/screenshots/fuchs

Zehn Fuchs-Screenshots; Originaldateinamen beschreiben die Ansichten.

| Datei | Inhalt |
| --- | --- |
| [Fuchs-CriticalFehler.png](assets/screenshots/fuchs/Fuchs-CriticalFehler.png) | PNG-Screenshot: CriticalFehler. |
| [Fuchs-Dashboard.png](assets/screenshots/fuchs/Fuchs-Dashboard.png) | PNG-Screenshot: Dashboard. |
| [Fuchs-ProcessApps1.png](assets/screenshots/fuchs/Fuchs-ProcessApps1.png) | PNG-Screenshot: ProcessApps1. |
| [Fuchs-ProcessApps2-Aufgaben.png](assets/screenshots/fuchs/Fuchs-ProcessApps2-Aufgaben.png) | PNG-Screenshot: ProcessApps2-Aufgaben. |
| [Fuchs-ProcessApps2-RunGate.png](assets/screenshots/fuchs/Fuchs-ProcessApps2-RunGate.png) | PNG-Screenshot: ProcessApps2-RunGate. |
| [Fuchs-ProcessApps2-ToolkitVersionen.png](assets/screenshots/fuchs/Fuchs-ProcessApps2-ToolkitVersionen.png) | PNG-Screenshot: ProcessApps2-ToolkitVersionen. |
| [Fuchs-SnapshotVergleich.png](assets/screenshots/fuchs/Fuchs-SnapshotVergleich.png) | PNG-Screenshot: SnapshotVergleich. |
| [Fuchs-SnapshotVergleich2.png](assets/screenshots/fuchs/Fuchs-SnapshotVergleich2.png) | PNG-Screenshot: SnapshotVergleich2. |
| [Fuchs-Suche.png](assets/screenshots/fuchs/Fuchs-Suche.png) | PNG-Screenshot: Suche. |
| [Fuchs-Toolkits.png](assets/screenshots/fuchs/Fuchs-Toolkits.png) | PNG-Screenshot: Toolkits. |

### assets/pdfs/vergleich

PDF-Fassungen der Funktionsvergleiche.

| Datei | Inhalt |
| --- | --- |
| [TWX-Analyzer-Funktionsvergleich-Commerzbank.pdf](assets/pdfs/vergleich/TWX-Analyzer-Funktionsvergleich-Commerzbank.pdf) | PDF-Variante des Vergleichs für die Commerzbank; ursprüngliche Variantenbezeichnung beibehalten. |
| [TWX-Analyzer-Funktionsvergleich-Commerzbank0.pdf](assets/pdfs/vergleich/TWX-Analyzer-Funktionsvergleich-Commerzbank0.pdf) | PDF-Variante des Vergleichs für die Commerzbank; ursprüngliche Variantenbezeichnung beibehalten. |
| [TWX-Analyzer-Funktionsvergleich-Commerzbank2.1.pdf](assets/pdfs/vergleich/TWX-Analyzer-Funktionsvergleich-Commerzbank2.1.pdf) | PDF-Variante des Vergleichs für die Commerzbank; ursprüngliche Variantenbezeichnung beibehalten. |
| [TWX-Analyzer-Funktionsvergleich-Commerzbank2.pdf](assets/pdfs/vergleich/TWX-Analyzer-Funktionsvergleich-Commerzbank2.pdf) | PDF-Variante des Vergleichs für die Commerzbank; ursprüngliche Variantenbezeichnung beibehalten. |
| [TWX-Analyzer-Funktionsvergleich-Commerzbank_LANG.pdf](assets/pdfs/vergleich/TWX-Analyzer-Funktionsvergleich-Commerzbank_LANG.pdf) | PDF-Variante des Vergleichs für die Commerzbank; ursprüngliche Variantenbezeichnung beibehalten. |
| [TWX-Analyzer-Funktionsvergleich-Fuchs-Perspektive.pdf](assets/pdfs/vergleich/TWX-Analyzer-Funktionsvergleich-Fuchs-Perspektive.pdf) | PDF-Fassung des Vergleichs aus Fuchs-Perspektive. |

### assets/pdfs/referenzen

Externe Referenzdokumente.

| Datei | Inhalt |
| --- | --- |
| [Salient-BPM-Cloud-Migration-Guide.pdf](assets/pdfs/referenzen/Salient-BPM-Cloud-Migration-Guide.pdf) | Abgelegter BPM-Cloud-Migrationsleitfaden von Salient Process. |

### beispiele/twx

TWX-Beispielarchive.

| Datei | Inhalt |
| --- | --- |
| [empty-project-8.6.8.24010.twx](beispiele/twx/empty-project-8.6.8.24010.twx) | TWX-Beispielarchiv mit 11 Einträgen, darunter XML-Objekte, Metadaten, eine Datei und zwei Toolkit-ZIPs. |

## Hinweise zum Bestand

- Die 36 vorhandenen Inhaltsdateien lagen zuvor im Repository-Stamm. Ihre Dateinamen bleiben erhalten; relative Quellenverweise berücksichtigen die neue Ablage.
- Die PDF-Dateien mit den Endungen Commerzbank0.pdf und Commerzbank2.pdf sind bytegleich. Beide Varianten bleiben erhalten. Aus den Dateinamen lässt sich keine verlässlich freigegebene Endfassung bestimmen.
- Der zusammengeführte Benutzerhandbuch-Export enthält Links auf ursprüngliche Einzeldokumente und Bilder, die neben der Sammeldatei fehlen. Diese bereits vorhandenen Verweise bleiben erhalten. Das ZIP-Archiv bewahrt die ursprüngliche docs/-Struktur.
- Relative Quellcodeverweise führen aus den Themenordnern in benachbarte TWX-Repositories. Ohne diese Checkouts sind die Ziele nicht verfügbar. Zeilenfragmente wie #L28 bezeichnen Quellzeilen und funktionieren nicht in jedem lokalen Markdown-Betrachter.
- twx-analyzer-v2 ist im Git-Index als Gitlink verzeichnet, fehlt aber im Arbeitsverzeichnis bereits seit vor dieser Neuordnung. Es gehört deshalb nicht zum oben dargestellten vorhandenen Dateibestand.
- .git/ enthält die Git-Verwaltung. .agents/, .aws/ und .codex/ sind vom Arbeitsbereich bereitgestellte Verzeichnisse und keine fachlichen Inhaltsordner.
- Es gibt hier derzeit keinen eigenen Build oder Testlauf. Bei Strukturänderungen werden Dateibestand, Binärprüfsummen und lokale Linkziele geprüft.
