# Erfahrungen und Probleme

Während des Aufbaus und Betriebs des HomeLab konnte ich praktische Erfahrungen in verschiedenen Bereichen sammeln.

Viele Probleme traten erst im laufenden Betrieb auf und mussten eigenständig analysiert und gelöst werden.

---

# Ordnerstruktur und Wartbarkeit

Eine der wichtigsten Erfahrungen war, dass eine saubere und übersichtliche Ordnerstruktur von Anfang an sehr wichtig ist.

Gerade bei mehreren Docker Containern erleichtert eine klare Struktur:
- die Wartung
- Updates einzelner Dienste
- die Fehlersuche
- spätere Erweiterungen

Mit zunehmender Anzahl an Diensten wurde deutlich, wie wichtig eine gute Organisation der Docker Compose Dateien und Konfigurationen ist.

---

# Sicherheit öffentlich erreichbarer Dienste

Vor dem Betrieb öffentlich erreichbarer Dienste wurde unterschätzt, wie viele automatisierte Zugriffe und Angriffe tatsächlich stattfinden.

Bereits nach kurzer Zeit wurden regelmäßig:
- Login Versuche
- Port Scans
- automatisierte Anfragen
- bekannte Exploits

in den Logs sichtbar.

Dadurch wurde schnell deutlich, wie wichtig Sicherheitsmaßnahmen wie:
- UFW
- Fail2Ban
- CrowdSec
- HTTPS
- VPN Zugriff

für öffentlich erreichbare Dienste sind.

---

# Fail2Ban und CrowdSec

## Beispiele

### Fail2Ban
- 165 geblockte IP-Adressen innerhalb der letzten 90 Tage

### CrowdSec
- 156 geblockte IP-Adressen innerhalb der letzten 4 Tage

Diese Erfahrungen haben gezeigt, wie wichtig zusätzliche Sicherheitsmaßnahmen auch bei kleineren privaten Servern sind.

---

# Honeypot Erfahrungen

Aus Interesse wurde zusätzlich der öffentlich erreichbare Dienst Krawl als Honeypot getestet.

Bereits innerhalb von 30 Tagen wurden insgesamt 182 Angriffsversuche erkannt.

Darunter:
- 130 Common Probes
- 34 SQL Injections
- 18 Command Injections

Dadurch wurde deutlich, wie häufig automatisierte Angriffe auf öffentlich erreichbare Dienste stattfinden.

---

# Updates und Wartung

Automatische Container Updates wurden getestet, führten jedoch teilweise zu Problemen mit einzelnen Containern oder Neustart-Schleifen.

Aus diesem Grund werden Updates inzwischen bewusst manuell durchgeführt, um mögliche Probleme direkt kontrollieren und beheben zu können.

Dadurch konnte ein besseres Verständnis für:
- Docker Updates
- Abhängigkeiten einzelner Dienste
- und Fehlersuche nach Updates

entwickelt werden.

## Konkretes Beispiel: Neustart-Schleife nach automatischem Update

**Problem:** Der Container von [Krawl](https://github.com/BlessedRebuS/Krawl) ging nach einem automatischen Image-Update in eine Neustart-Schleife.

**Analyse:** 
- `docker logs krawl`
- `docker compose config`
- Änderungen im Changelog des Images geprüft

**Lösung:** Die Compose-Datei wurde an das neue Update angepasst.

**Erkenntnis:** Docker-Images nicht automatisch updaten, da es sonst zu unnötigen Neustart-Schleifen kommen kann.

---

# Monitoring und Benachrichtigungen

Durch die Nutzung von Discord Webhooks wurde gelernt, wie hilfreich zentrale Benachrichtigungen für den Betrieb mehrerer Dienste sein können.

Probleme oder Ausfälle können dadurch schneller erkannt werden, ohne alle Dienste manuell kontrollieren zu müssen.

---

# Persönlicher Lernfortschritt

Durch das HomeLab konnten praktische Erfahrungen gesammelt werden in:
- Linux Administration
- Docker und Docker Compose
- Netzwerkgrundlagen
- Reverse Proxy Konfiguration
- HTTPS und Domains
- VPN Zugriff
- Monitoring
- Sicherheitsmaßnahmen
- Fehlersuche und Problemlösung

Viele dieser Themen konnten erst durch praktische Nutzung und den Umgang mit realen Problemen besser verstanden werden.