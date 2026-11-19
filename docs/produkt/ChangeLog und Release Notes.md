# Release notes - twx-Analyzer

## Version 2.3.8

Release Datum: 25.10.2024

Story:

[TA-417]: Das Backend muss auf eine veränderte Plugin-Liste reagieren können    
[TA-416]: Verwalten der Liste der installierten Plugins über Helm Chart    
[TA-462]: Der technische User für BAW wird über Helm konfiguriert.    

Hinweis:

Kann aber noch über Frontend verändert werden. Zukünftig wird das dann nicht mehr gehen
Update auf MongoDB 8.0.1
Das Helm Chart wird zukünftig über Artifactory geliefert
https://twxsolutions.jfrog.io/artifactory/twx-analyzer-helm-local/twx-analyzer-2.3.8.tgz

## Version 2.3.7

Release Datum: 19.08.2024

Story:

[TA-103]: Plugin zu Process App oder Toolkit Zuordnung werden wird nicht mehr gelöscht, wenn der letzte Snapshot einer ProcessApp gelöscht wird.  
[TA-107]: Ähnliche Objektebezeichnungen als Fehler melden. Suche nach Datentypen die sich namentlich sehr ähneln. Zum Beispiel durch mehrfaches „Discovering“ der SoapService-Schnittstellen (WSDLs) können immer gleichnamige oder leicht abweichende (Name2\) Objektebezeichnungen entstehen. Die Suche umspannt die ProcessApp und alle Toolkits außer den SystemToolkits.  
[TA-114]: Entfernung von Material Design Elementen.  
[TA-179]: Toolkit-Konfliktergebnisse werden nicht mehr angezeigt, wenn das entsprechende Plugin in der Zuordnung von Process Apps oder Toolkits deaktiviert ist.  
[TA-325]: Einführung einer neuen Funktion zur Ermittlung nicht referenzierter (Orphan) Toolkits. Diese Funktion hilft bei der Bereinigung und Verwaltung von Toolkits.  
[TA-392]: Verbesserungen der Plugin zu Process App oder Toolkit Zuordnung: Spaltenklick-Funktionalität hinzugefügt, um die Benutzererfahrung zu optimieren. Performance-Optimierungen vorgenommen, insbesondere bei vielen Einträgen, um eine schnellere Verarbeitung zu gewährleisten.  
[TA-447]: Korrektur eines Fehlers, bei dem das Process-Acronym in der aktiven Snapshot-Anzeige fehlte. Diese Anpassung stellt sicher, dass alle relevanten Informationen korrekt angezeigt werden.  
[TA-448]: Behebung eines Fehlers, der auftrat, wenn keine Auswahl beim automatischen Download getroffen wurde.  
[TA-449]: Anpassung des Symbols für „running“ im Diff-History-Bereich, um die Visualisierung des Status klarer und intuitiver zu gestalten.  
[TA-471]: Aktualisierung des XML-Parsings und Anpassung auf die neue BAW-Version. Diese Änderung verbessert die Kompatibilität und Zuverlässigkeit bei der Verarbeitung von XML-Daten.  


## Version 2.3.6 

Release Datum: 28.03.2024

Story:

[TA-423]: Permanentes ignorieren von Findings über das Frontend  
[TA-442]: Snapshot History Anzahl begrenzen  
[TA-406]: Library Updates \+ Security Fixes  

Hinweise:

WICHTIG: Das SECERET in der security.properties muss 512 Zeichen lang sein.
Ich bin noch dabei eine Verbesserung zu finden, dass dies zukünftig nicht mehr nötig ist.
Da bei euch vermutlich bei einem Update noch immer die DB gelöscht wird, denkt an das Backup der Plugin zu Toolkit Zuordnungen unter Settings -\> Database
Es gibt nun einen Snapshot Analyse Historie. Per Default werden 2 Snapshots aufbewahrt.


## Version 2.3.5

Release Datum: 08.01.2023

Story:

[TWXASD-70]: "Unused Variable" Plugin does not check the bindings and configurations of the views - false positive  
[TWXASD-69]: Fulltext search does not search event handlers (like load(), view(), change()) of the views

## Version 2.3.4

Release Datum: 12.12.2023

Story:

[TA-408]: Missing error message at snapshot diff when Xpath invalid  
[TWXASD-66]: Filter capability for ignorable tags  
[TWXASD-61]: Backup and restore for plugin to tk/app configuration  
[TA-409]: Ignorable tags now with validation if input is valid xpath  
[TWXASD-57]: branchId of the package.xml is now removable via ignorable tag configuration


## Version 2.3.3

Release Datum: 01.12.2023

Story:

[TWXASD-54]: - Improvements - TWX-Analyzer Snapshot compare exceeds browser memory  
[TA-395]: Fulltextsearch also for Toolkits including frontend restructure

Hinweise:

Die Problematik von großen Diff ist verbessert aber eventuell noch nicht ganz gelöst.
Nun können im Frontend Anzeigelimits konfiguriert werden
Es kann gesteuert werden wie viele Linien git beim Diff für und nach dem Diff speichert
Die Volltextsuche für Toolkits ignoriert System-Toolkits
Bei der Volltextsuche wird nun durch Strg \+ Klick ein weiteres Element ausgewählt


## Version 2.3.2

Release Datum: 27.09.2023

Story:
[TA-393] - Volltextsuche für BPDs aktivieren  
[TA-394] - “Token is not valid” häufig im Log  
[TA-385] - Orphan Toolkits Generierung auf Knopfdruck um Process Center nicht zu überlasten  
[TA-385] - Orphan Toolkits entfernen nicht relevanter Elemente für Beta Version  
[TA-386] - Snapshot Diff: Toolkitvergleich wird nun richtig in Git angelegt  
[TA-387] - System Toolkits werden aus dem Snapshot Diff ausgeschlossen  
[TA-393] - Ignoriere System Toolkits beim Snapshot-Diff (Kleiner performance boost)  
[security] - JGit version Security Update

Hinweise:

Da wir nun auch die Volltextsuche (innerhalb einer ProcessApp) für BPDs (Prozess) eingebaut haben, müssen bereits analysierte nochmals analysiert werden, damit der Index in der Datenbank aufgebaut werden kann. Hierfür gibt es in der Snapshot-Übersicht ein „Reanalyze“ button:

Wir haben uns, um eine bessere Performance zu erreichen, dazu entschlossen die System-Toolkits aus dem Snapshot Diff zu entfernen. Wir gehen davon aus, dass diese Toolkits sowieso nie manuell editiert werden (darf nie passieren). Eine Aktualisierung der Version würde aber an der entsprechenden package.xml-Datei angezeigt.



## Version 2.3.1

Release Datum: 04.09.2023

Story:

[TA-346] - Git Diff zwischen zwei Snapshots  
[TA-325] - Ermitteln nicht referenzierte Toolkit-Snapshots (API)  
[TA-345] - Versionsinformation im Frontend anzeigen  
[TA-360] - Log Folder unter /log  
[TA-366] - Frontend Umstrukturierung nutzt Tabs

Hinweise:

Für den Snapshot-Vergleich habe ich umfassende Informationen in der Online-Dokumentation des twx-Analyzers hinterlassen.
Anbei eine Kopie dieser Dokumentation im Markdown-Format.

## Version 2.3.0

Release Datum: 04.04.2023

Story:

[TA-160] - CICD Controller für die Abfrage via Acronym  
[TA-270] - Update auf Angular 13  
[TA-286] - Junit 5 Migration  
[TA-287] - Frontend ist jetzt via OpenShift über eine Datei Konfigurierbar  
[TA-288] - Update auf Java 17 und Optimierung  
[TA-321] - BAW Download über neue IBM API  
[TA-238] - Shutdown Endpunkt nun erreichbar und per Default AKTIV.

## Version 2.2.2 

Release Datum: 18.12.2022

Story:

Volltextsuche
Edge Browser Probleme mit dem Login

## Version 2.2.0 

Release Datum: 18.12.2022

Story:

Openshift Readyness

## Version 2.1.0 

Release Datum: 23.12.2021

Story:

[TA-2] - Login mit Active Directory Anbindung.  
[TA-124] - Bessere Quellenanzeige, wenn ein Finding zu einem Toolkit gehört.  
[TA-232] - Aktualisierung auf Spring Boot 2.6.1  
[TA-251] - Log4j Vulnerability - Aktualisierung auf 2.17.0  
[TA-189] - Volltextsuche für ausgewählte ElementTypen (MongoDB 4.2 benötigt)  
[TA-218] - Link bei Start in Konsole jetzt korrekt zwischen http und https unterschieden  
[TA-238] - Shutdown Endpunkt nun erreichbar und per Default AKTIV.

Defekts:

[TWXASD-32] - Nullpointer bei der Analyse der ISF Dateien

Deprecation:
Plugin „Entry und Exit Logging“: Seit BAW 19 kann der ServiceName ermittelt werden, daher ist das Entry und Exit Logging
Plugin nicht mehr aktuell und wird zunächst nicht weiterentwickelt. Plugin „ServiceName Variable“: Seit BAW 19 kann der 
ServiceName ermittelt werden, daher ist das Plugin obsolete und wird nicht weiterentwickelt.

## Version 2.0.7

Release Datum: 21.03.2021

Story:

[TA-68] - CI/CD Api Calls besser Strukturieren  
[TA-177] - CI/CD Controller Update und neue Struktur umsetzen. Siehe neues OpenAPI File in der Dokumentation  
[TA-147] - 2.0.6 - CobaFin | Plugin 5 | Object Naming Conditions  
[TA-167] - Update auf Java 11 und Verbesserungen

Defekts:

[TWXASD-27] - Nicht gefundene Variable bei External Service Calls  
[TWXASD-26] - Nicht lesbare Pfade bei Zielangabe  
[TWXASD-28] CI/CD: 0-Fehler bei SnapshotId-Analyse  

## Version 2.0.6.1

Story:

[TA-138] - CobaFin | Plugin 3 | Variable Description Missing  
[TA-139] - CobaFin | Plugin 4 | Snapshot Naming  
[TA-157] - PIA | Plugin 1 | Exception Handling  
[TA-153] - Update auf neuste Spring Boot Version  
[TA-118] - Deprecation Warnung beim Start eines Elements (Service, BPD) im Web Process Designer  
[TA-157] - Rest API zur Einbindung in automatischen Build. Siehe yaml-Datei in der Online Dokumentation unter „Installation -\> Integration in build automation system“. Derzeit ist der Aufruf nur mit Snapshot- und ProcessApp-IDs möglich. Im nächsten Release wird auf Acronyme umgestellt.  
[TA-3] - Theme Umstellungen ermöglichen

Defekts:

[TA-118] - Speichern der Einstellungen mit neuer Spring Version führt zu einem Fehler. Siehe installation-readme.md für Anweisungen  
[TA-152] - Nullpointer bei der Erzeugung der Liste der UCA Abhängigkeiten

Deprecation:

Öffnen Elemente im WebPD - Da derzeit nicht für alle Services das Öffnen der Elemente im WebPD möglich ist und IBM hierfür auch keinen Support bietet, wird diese Funktion derzeit eine Deprecation Warnung erzeugen. Mit dem nächsten Release wird diese Funktion deaktiviert, bleibt aber zunächst aktivierbar.

## Version 2.0.5.1

Story:
[TA-138] - CobaFin | Plugin 1 | Element Description Missing  
[TA-139] - CobaFin | Plugin 2 | History Documentation

Defekts:

[TA-113] - Toolkit-Auswahl greift nicht immer, wodurch Analysen für Plugins getrennt durchgeführt werden müssen.  
[TA-129] - Variablen werden als „unused Variables“ betrachtet obwohl Sie laut WebPD verwendet werden (DIKO \+ RUKOFF)  
[TA-136] - DiPro | Falschmeldung durch twx Analyzer | UCA Variablen Mapping werden als unused erkannt.  
[TA-152] - Nullpointer bei Erzeugung der UCA-Abhängigkeiten

Deprecation:
Update siehe Release 2.0.6