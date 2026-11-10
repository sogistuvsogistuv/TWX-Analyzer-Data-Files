**twx Solutions GmbH**

Entscheidungsgrundlage für die Commerzbank

BAW-Regelbetrieb

# Negativmerkmale

## TWX Analyzer v2 · Fuchs IT-Consulting

- **Datenbank.** Der Betrieb muss MariaDB als separate Komponente einrichten, sichern und aktualisieren.
- **Quellcode.** Öffentliches Open-Source-Repository unter Apache 2.0.
- **Projekthistorie.** Die öffentliche Git-Historie reichte zum Prüfstand rund drei Wochen zurück. Ein längerfristiger Einsatz im Enterprise-Betrieb ist nicht belegt.
- **Anbieterform.** Selbständiger IT-Berater. Das Impressum nennt eine Einzelperson als Inhaber und enthält keine Handelsregisternummer.
- **OpenShift.** Fuchs dokumentiert den Betrieb mit Podman. Für OpenShift muss der Betreiber die Bereitstellung von Frontend, Backend und MariaDB samt Netzwerk und Speicher konfigurieren und testen, etwa mit einem eigenen Helm-Chart.

## TWX Analyzer & Migrator · Salient Process

- **Funktionsnachweise.** Die öffentlichen Unterlagen dokumentieren vor allem Migrationsfunktionen. Der aktuelle Funktionsumfang für regelmäßige BAW-Analysen bleibt offen.
- **Betriebsmodell.** Ein Online-Angebot mit TWX-Upload ist historisch dokumentiert.
- **Migrationsfokus.** Salient beschreibt den Wechsel von IBM BPM zu BAW in der IBM Cloud. Als Anlass nennt der Anbieter das Supportende von BPM 8.6.x im Jahr 2020.

## TWX Code Analyzer · Dosvak

- **Speicherung.** Die geprüfte WAR speichert TWX-Uploads, Berichte und Einstellungen als Dateien auf dem Server. Eine separate Datenbank ist nicht erforderlich. Persistenter Speicher und Dateisicherungen sind einzuplanen.
- **Quellcode.** Öffentliches Open-Source-Repository. Der geprüfte Quellstand steht unter Apache 2.0.
- **OpenShift.** Die WAR benötigt einen kompatiblen Servlet-Container. Für OpenShift muss der Betreiber ein passendes Container-Image und die Deployment-Konfiguration bereitstellen, etwa mit einem eigenen Helm-Chart.

