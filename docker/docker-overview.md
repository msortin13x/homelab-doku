# Docker & Container

Zur Verwaltung der einzelnen Dienste wird Docker in Kombination mit Docker Compose verwendet.

Konfiguration und Laufzeitdaten sind bewusst voneinander getrennt.

```text
/docker
 ├── jellyfin/          docker-compose.yml, .env
 ├── vaultwarden/
 ├── kavita/
 ├── traefik/
 └── ...

/docker-data
 ├── traefik/           traefik.yml, config.yml, acme.json
 ├── crowdsec/          Konfiguration, Datenbank, Parser
 ├── kavita/
 └── ...
```

Unter `docker/` liegen ausschließlich die Compose-Dateien der einzelnen Dienste.
Alle persistenten Daten und dienstspezifische Konfigurationsdateien liegen
getrennt davon unter `docker-data/`.

Dadurch lassen sich die einzelnen Anwendungen voneinander getrennt verwalten und einfacher aktualisieren oder erweitern.

---

# Trennung von Konfiguration und Daten

Ursprünglich lagen Compose-Dateien und Laufzeitdaten eines Dienstes gemeinsam in einem Verzeichnis.
Die Struktur wurde nachträglich getrennt.

Der Grund dafür ist die Versionierbarkeit: `docker/` enthält nur Compose-Dateien und lässt sich damit vollständig in einem
privaten Git-Repository verwalten, jeweils zusammen mit einer `.env.example`, die die benötigten Variablen ohne deren Werte dokumentiert.
Die eigentlichen `.env`-Dateien mit Secrets sowie alle Laufzeitdaten unter `docker-data/` bleiben davon ausgenommen.

In der alten Struktur hätten Datenbanken, Zertifikate und Secrets im selben Verzeichnis gelegen wie die zu versionierenden Dateien.

Perspektivisch soll darüber ein Deployment über GitHub möglich sein, bei dem Konfigurationsänderungen per `git pull` auf den Server gelangen,
ohne Laufzeitdaten zu berühren.

**Offener Punkt:** Die Compose-Dateien enthalten derzeit absolute Pfade zu `docker-data/`. Für ein tatsächlich portables Deployment
müssten diese über eine Variable in der `.env` aufgelöst werden.

---

# Docker Compose Struktur

Beim Aufbau der Containerstruktur war besonders wichtig:
- einfache Wartbarkeit
- übersichtliche Struktur
- getrennte Verwaltung einzelner Dienste
- einfache Erweiterbarkeit
- möglichst unkomplizierte Updates

---

# Warum Docker verwendet wird

Docker ermöglicht es, Dienste unabhängig vom eigentlichen System in separaten Containern bereitzustellen.

Für die Größe des HomeLab reicht Docker vollständig aus und ermöglicht eine übersichtliche Verwaltung der einzelnen Container ohne zusätzlichen Verwaltungsaufwand durch komplexe Orchestrierungslösungen wie Kubernetes.

Dadurch ergeben sich unter anderem folgende Vorteile:
- Dienste können unabhängig voneinander aktualisiert oder neugestartet werden
- neue Anwendungen lassen sich einfacher testen und hinzufügen
- Konfigurationen einzelner Dienste bleiben besser getrennt
- einzelne Container lassen sich bei Problemen einfacher austauschen oder neu erstellen

---

# Verwendete Container

## Öffentlich erreichbare Dienste
- Jellyfin
- Vaultwarden
- Kavita
- SFTPGo

Die Erreichbarkeit wird innerhalb der [Netzwerkübersicht](/network/network-overview.md#öffentliche-dienste) erläutert.

## Interne Dienste
- Dockhand
- Uptime Kuma
- CrowdSec
- CrowdSec WebUI

---

# Beispiel: Traefik Labels

Jeder öffentlich erreichbare Dienst erhält Labels in seiner `docker-compose.yml`, über die Traefik das Routing und die HTTPS-Zertifikate automatisch übernimmt.

Beispiel (anonymisiert):
```yaml
services:
    beispiel-dienst:
      image: beispiel/dienst:latest
      container_name: beispiel-dienst
      restart: unless-stopped
      networks:
        - proxy
      labels:
        - "traefik.enable=true"
        - "traefik.http.routers.beispiel-dienst.rule=Host(`beispiel.domain.xyz`)"
        - "traefik.http.routers.beispiel-dienst.entrypoints=websecure"
        - "traefik.http.routers.beispiel-dienst.tls.certresolver=cloudflare"

networks:
  proxy:
    external: true
```

Dadurch übernimmt Traefik automatisch das Routing der Subdomain sowie die Ausstellung und Erneuerung des HTTPS-Zertifikats über Cloudflare, ohne dass der Dienst selbst etwas davon wissen muss.

---

# Verwaltung und Updates

Container-Updates werden bewusst manuell durchgeführt.

Updates erfolgen:
- über Dockhand
- oder manuell über `docker compose pull`

Automatische Updates wurden getestet, führten jedoch teilweise zu Problemen mit einzelnen Containern oder Neustart-Schleifen.

Aus diesem Grund wurde sich bewusst gegen automatische Container-Updates entschieden.

---

# Weitere Informationen
Weitere Informationen zu Monitoring und Benachrichtigungen befinden sich in der Datei [monitoring.md](/docker/monitoring.md).

Weitere Informationen zu Backups und Wartung befinden sich in der Datei [backup-maintenance.md](/docker/backup-maintenance.md).