# Sicherheitsmaßnahmen

Da einzelne Dienste öffentlich erreichbar sind, war die Absicherung des Servers ein wichtiger Bestandteil beim Aufbau des HomeLab.

Dabei wurde besonders darauf geachtet:
- möglichst wenige Ports öffentlich freizugeben
- interne und öffentliche Dienste voneinander zu trennen
- Zugriffe zu überwachen
- unerwünschte Zugriffe möglichst früh zu erkennen

---

# Reverse Proxy und HTTPS

Öffentlich erreichbare Dienste laufen über Traefik als Reverse Proxy.

Die Verwaltung der Domains und HTTPS-Verbindungen erfolgt über Cloudflare.

Dadurch sind die Dienste ausschließlich verschlüsselt über HTTPS erreichbar.

---

# Tailscale VPN

Interne Verwaltungsdienste sind nicht direkt öffentlich erreichbar.

Der Zugriff erfolgt entweder lokal innerhalb des Heimnetzwerkes oder extern über ein VPN mit Tailscale.

Dadurch müssen für interne Dienste keine zusätzlichen Ports öffentlich freigegeben werden.

---

# UFW Firewall

Zur Absicherung des Servers wird UFW verwendet.

Nicht benötigte Ports sind standardmäßig blockiert.
Freigegeben werden nur die tatsächlich benötigten Dienste.

Zusätzlich werden einige Dienste bewusst nur lokal oder innerhalb bestimmter Netzwerke erreichbar gemacht.

<a href="../screenshots/ufw_status.png">
  <img src="../screenshots/ufw_status.png" alt="UFW Status" width="50%">
</a>

---

# Fail2Ban

Fail2Ban wird verwendet, um Brute-Force Angriffe auf einzelne Dienste zu erkennen und IP-Adressen automatisch zu blockieren.

Besonders geschützt werden:
- SSH
- SFTPGo

Da SSH ausschließlich privat genutzt wird, wurden dafür strengere Regeln verwendet als für den gemeinsam genutzten SFTP-Dienst.

Zusätzlich sendet Fail2Ban Benachrichtigungen über Discord Webhooks.

## Beispiel: Jail-Konfiguration

Anonymisierter Ausschnitt aus `jail.local` für den SSH-Schutz:

```ini
[sshd]
enabled   = true
port      = ssh
filter    = sshd
logpath   = /var/log/auth.log
maxretry  = 3
findtime  = 10m
bantime   = 1h
```

---

# CrowdSec

CrowdSec wird zur zusätzlichen Überwachung wichtiger Zugriffe verwendet.

Dadurch können auffällige Zugriffe und potenzielle Angriffe schneller erkannt werden.

Zur einfacheren Verwaltung und Übersicht wird zusätzlich die CrowdSec WebUI verwendet.
<a href="../screenshots/crowdsec_webui.png">
  <img src="../screenshots/crowdsec_webui.png" alt="CrowdSec WebUI" width="50%">
</a>

Auch CrowdSec sendet Benachrichtigungen über Discord Webhooks.

---

# Weitere Informationen

Weitere Informationen zum allgemeinen Netzwerkaufbau befinden sich in der Datei [network-overview.md](network-overview.md).