**twx Solutions GmbH**

Entscheidungsgrundlage für die Commerzbank

BAW-Regelbetrieb

# Ergänzungen zum Vergleich

## Stärken der Alternativen

**Fuchs IT-Consulting**

Globales Toolkit-Inventar mit Nutzungsnachweisen.

**Dosvak**

Integrierte Quality Gates mit Grenzwerten je Regel.

**Salient Process**

IBM-Cloud-Migrationsplanung und Artefakttransformation.

## Weitere Funktionen

- **Autoload**

  Ruft den neuesten gespeicherten Snapshot des gewählten BAW-Tracks ab.

- **Gezielte Prüfausnahmen in BAW**

  Ein BAW-Tag kann ein Element von ausgewählten Prüf-Plugins ausnehmen. Bei Fuchs greift der Tag-Ausschluss für alle Befunde des Elements.

- **TODO-Übersicht**

  Erfasst bei passendem Format auch Verfasser, Empfänger und Datum.

- **Lokalisierungsressourcen**

  Zeigt Übersetzungstexte je Sprache und die Stellen, an denen sie verwendet werden.

- **Prüfregeln je App und Toolkit**

  Für Apps eines Mandanten lässt sich getrennt festlegen, welche Prüfregeln für die App selbst und welche für ihre eingebetteten Toolkits gelten.

- **Umgebungsvariablen zentral prüfen**

  Zeigt Werte aus App und eingebetteten Toolkits in einer gemeinsamen Tabelle mit CSV-Export.

- **Mandantenverwaltung per REST**

  Active-Directory-Gruppen und Prozessanwendungen lassen sich Mandanten zuordnen.

- **Selektive Datensicherung per REST**

  MongoDB-Sammlungen lassen sich einzeln als JSON oder gesammelt als ZIP sichern. Unterstützte Bestände lassen sich wieder importieren.

## Anbieter und Betriebsmodell

### Fuchs IT-Consulting

- **Anbieter.** Selbständiger IT-Berater, der unter eigenem Namen auftritt.
- **Quellcode.** Öffentliches Repository. Geprüfter Quellstand unter Apache 2.0.
- **Datenbank.** MariaDB.
- **Produktreife.** Die öffentliche Git-Historie reicht zum Prüfstand rund drei Wochen zurück. Ein längerfristiger Einsatz im Enterprise-Betrieb ist nicht belegt.

### Dosvak

- **Anbieter.** Dosvak LLC mit Hauptsitz in den USA.
- **Quellcode.** Öffentliches Repository. Geprüfter Quellstand unter Apache 2.0.
- **Betrieb.** Eigenbetrieb als Serveranwendung (WAR) möglich.
- **Speicherung.** Die geprüfte Serveranwendung speichert TWX-Uploads, Berichte und Einstellungen als Dateien auf dem Server. Sie benötigt keine separate Datenbank.

### Salient Process

- **Anbieter.** Salient Process Inc. (USA).
- **Betrieb.** Cloudanwendung mit TWX-Upload, historisch dokumentiert. Heutiger Kundenzugang und Eigenbetrieb sind offen.
- **Einsatz.** Der Analyzer bewertet den Migrationsaufwand. Der Migrator automatisiert Änderungen beim Wechsel von IBM BPM zu BAW. Der IBM-Support für BPM 8.5.x endete 2019, für 8.6.x 2020.

