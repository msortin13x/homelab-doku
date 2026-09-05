# Backup und Wartung

Für wichtige Daten werden zusätzliche Backups außerhalb des eigentlichen Servers erstellt.

Das RAID-5-System dient hauptsächlich der Ausfallsicherheit der Festplatten und ersetzt kein eigenes Backup.

---

# Backup Strategie

Wichtige Dateien und Konfigurationen werden zusätzlich gesichert auf:
- einem persönlichen PC
- einer externen Festplatte
- einem USB-Stick

Damit wird das 3-2-1-Backup-Prinzip umgesetzt: mindestens drei Kopien der Daten, auf zwei unterschiedlichen Medientypen, wovon eine Kopie extern (außerhalb des Servers) aufbewahrt wird.

Dadurch bleiben wichtige Daten auch bei Problemen mit dem Server oder dem RAID-System weiterhin verfügbar.

Zusätzlich werden die Docker-Compose-Dateien in einem privaten Git-Repository versioniert.
Dadurch ist neben der reinen Sicherung auch nachvollziehbar, wann und warum eine Konfiguration geändert wurde.
Laufzeitdaten und `.env`-Dateien mit Secrets sind davon ausgenommen und über die oben genannten Backups abgedeckt.

---

# RAID 5

Als Speicherlösung wird ein USB RAID Enclosure mit vier 12 TB HDDs im RAID-5-Verbund verwendet.

Der RAID-Verbund ermöglicht eine bessere Ausfallsicherheit bei einem Festplattenausfall und stellt gleichzeitig zentralen Speicherplatz für die einzelnen Dienste bereit.

---

# Wartung der Container

Docker Container werden regelmäßig überprüft und bei Bedarf manuell aktualisiert.

Automatische Updates wurden getestet, führten jedoch teilweise zu Problemen mit einzelnen Containern oder Neustart-Schleifen.

Aus diesem Grund werden Updates bewusst kontrolliert durchgeführt.

---

# Systemupdates

Auch Systemupdates werden manuell durchgeführt.

Dadurch können mögliche Probleme nach Updates schneller erkannt und direkt überprüft werden.

---

# Ziel der Wartung

Beim Betrieb des HomeLab war besonders wichtig:
- kontrollierte Updates
- möglichst einfache Wiederherstellung wichtiger Daten
- stabile Laufzeit der Dienste
- übersichtliche Verwaltung der Container

---

# Weiterführend

- [Weiter: Erfahrungen & Probleme](../troubleshooting/lessons-learned.md) - Aufgetretene Probleme und Lösungen
- [Zurück: Monitoring](monitoring.md) - Überwachung der Dienste und Benachrichtigungen
- [Zurück zur Übersicht](../README.md)
