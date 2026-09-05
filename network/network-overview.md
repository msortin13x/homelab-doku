# Netzwerkübersicht

Der HomeLab Server ist per LAN mit einer FRITZ!Box verbunden und verwendet eine statische lokale IP-Adresse.

Neben dem lokalen Zugriff können einzelne Dienste auch extern erreicht werden. Dabei wird zwischen öffentlichen Diensten und internen Verwaltungsdiensten unterschieden.

## Aufbau

```mermaid
flowchart TD
    A[Internet] --> B[Cloudflare]
    B -->|nur Cloudflare-Ranges| C1["FRITZ!Box"]
    C1 --> C[HomeLab Server]
    C --> D[Traefik Reverse Proxy]
    D --> BO[CrowdSec Bouncer]
    BO -->|erlaubt| E[Öffentliche Dienste]
    BO -->|gesperrt| X[403 Forbidden]

    H[Client im Heimnetz] -->|Lokales Netzwerk| G
    I[Client unterwegs] -->|Tailscale VPN| G
    G[Interne Verwaltungsdienste]
    C --> G
```

---

# Öffentliche Dienste

Öffentlich erreichbare Dienste laufen über eine eigene Domain mit Subdomains.

Beispiele:
- jellyfin.domain.xyz
- vaultwarden.domain.xyz
- kavita.domain.xyz

Der externe Zugriff erfolgt über einen Reverse Proxy mit HTTPS-Verschlüsselung.

## Verwendete Komponenten

| Komponente | Zweck |
|---|---|
| Cloudflare | DNS Verwaltung und HTTPS |
| Traefik | Reverse Proxy |
| CrowdSec | Erkennung und Sperrung auffälliger Zugriffe |
| Docker | Bereitstellung der Dienste|

---

# Interne Dienste

Verwaltungsdienste sind nicht direkt öffentlich erreichbar.

Der Zugriff erfolgt entweder lokal innerhalb des Netzwerkes oder, falls nötig, extern über ein VPN mit Tailscale.

Dadurch können interne Dienste sicher aus dem Heimnetz oder von unterwegs erreicht werden, ohne zusätzliche Ports öffentlich freizugeben.

## Beispiele interner Dienste

- Dockhand
- Crowdsec WebUI
- Uptime Kuma

---

# Ziel des Netzwerkaufbaus

Beim Aufbau des Netzwerks war mir besonders wichtig:
- Trennung zwischen öffentlichen und internen Diensten
- Sichere externe Erreichbarkeit
- HTTPS für öffentliche Dienste
- Öffentliche Ports nur über Cloudflare erreichbar
- Einfache Wartbarkeit der Container
- Zugriff auf Verwaltungsdienste über das lokale Netzwerk oder über VPN

---

# Weiterführend

- [Weiter: Sicherheitsmaßnahmen](security.md) - Firewall, CrowdSec, Fail2Ban und Zugriffsschutz
- [Zurück zur Übersicht](../README.md)