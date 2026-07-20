# Docker & Container

Zur Verwaltung der einzelnen Dienste wird Docker in Kombination mit Docker Compose verwendet.

Die einzelnen Dienste besitzen jeweils eigene Docker Compose Verzeichnisse.

```text
/docker
 ├── jellyfin/
 ├── vaultwarden/
 ├── kavita/
 ├── traefik/
 └── ...
```

Dadurch lassen sich die einzelnen Anwendungen voneinander getrennt verwalten und einfacher aktualisieren oder erweitern.

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