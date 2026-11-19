# Arbeitsregeln für TWX-Analyzer-Data-Files

## Einstieg

Lies zuerst die [Repository-Map REMOMAP.md](REMOMAP.md). Sie beschreibt die Ablage und führt die vorhandenen Dateien auf. Der [README](README.md) bietet den kurzen Einstieg.

Dieses Repository enthält Dokumente, Bilder, PDFs und Beispielarchive. Es besitzt derzeit keinen eigenen Anwendungs-Build und keine automatisierte Testsuite.

## Inhalte bearbeiten

- Erfinde keine Inhalte. Kennzeichne unbekannte oder nicht verifizierte Aussagen ausdrücklich.
- Prüfe Produktbehauptungen anhand der referenzierten Quellen. Vorhandene Vergleichstexte sind keine Bestätigung des aktuellen Funktionsumfangs.
- Erhalte vorhandene Änderungen. Prüfe vor jeder Umstrukturierung den Repository-Pfad und den Git-Status.
- Lege neue Dateien im passenden Themenordner aus der Map ab. Aktualisiere bei neuen, verschobenen oder umbenannten Dateien auch REMOMAP.md und betroffene Verweise.
- Erhalte bestehende Dateinamen und Varianten, sofern die Aufgabe keine Umbenennung oder Zusammenführung verlangt. Lösche auch identische PDF-Varianten nicht ohne ausdrücklichen Auftrag.
- Verwende für neue Markdown-Dateien UTF-8 und relative Links. Erhalte beim Verschieben bestehende Kodierungen und Zeilenenden. Die Release-Hinweise in docs/produkt/ sind derzeit CP1252-kodiert.
- Ändere AGB-Texte nur bei einem ausdrücklichen inhaltlichen Auftrag.
- Bewahre Bilder, PDFs, ZIP- und TWX-Archive beim Umordnen bytegleich. Entpacke Archive nur, wenn die Aufgabe dies benötigt.

## Verweise und Nachbarprojekte

Verweise auf Backend, Web, Parser und Helm setzen entsprechende benachbarte Checkouts voraus. Prüfe deren tatsächliche Pfade, bevor du Aussagen zum Code ableitest. Änderungen an diesen Repositories benötigen einen passenden Auftrag und die dort geltenden Regeln.

Der kombinierte Benutzerhandbuch-Export enthält bereits unaufgelöste Links aus seiner ursprünglichen Dokumentationsstruktur. Die Einzeldateien liegen teilweise im ZIP-Export. Erfinde keine Ersatzdateien und ändere Verweise nur anhand überprüfter Ziele.

Der Git-Index verzeichnet twx-analyzer-v2 als Gitlink. Der Pfad fehlte bereits vor der Neuordnung. Stelle ihn nicht beiläufig wieder her und entferne den Indexeintrag nicht als Teil einer Dokumentationsänderung.

## Prüfung

Prüfe nach einer Umordnung die Vollständigkeit des Dateibestands, die unveränderten Binärdateien und die betroffenen lokalen Linkziele. Prüfe neue Dokumentationslinks einschließlich ihrer Überschriftenanker. Unterscheide bestehende Linkprobleme von neu entstandenen Fehlern. Halte Änderungen an Quellinhalten von reinen Pfadanpassungen getrennt nachvollziehbar.
