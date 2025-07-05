---
title: CI/CD
description: 
published: true
date: 2025-05-08T11:47:58.963Z
tags: 
editor: markdown
dateCreated: 2025-05-08T11:47:58.963Z
---

# 🚀 CI/CD

In diesem Abschnitt wird die CI/CD-Infrastruktur beschrieben.

---

# 📈 CI/CD Build- und Deployment-Fluss

Hier eine vereinfachte grafische Übersicht, wie die Workflows je nach Tag-Format ausgelöst werden:

```plaintext
Push Tag →
├── v-dev-client-*          → Build Client → (lokale Ablage)
├── v-dev-server-*          → Build Server → (lokale Ablage)
├── v-dev-both-*            → Build Server → Build Client → (lokale Ablage)
├── v-beta-client-itchio-*  → Build Client → Upload nach Itch.io (Butler)
├── v-beta-client-steam-*   → Build Client → Vorbereitung Steam Upload
├── v-beta-server-prepare-* → Build Server → Vorbereitung Wartungsmodus
├── v-beta-server-live-*    → Build Server → Server Liveschaltung
├── v-beta-both-*           → Build Server → Build Client → Vollständiger Rollout
```

# 🛠 Schrittweiser Ablauf

- Tag wird gepusht auf `dev` oder `main`
- Runner erkennt anhand des Tag-Musters den passenden Workflow
- Build-Prozess wird automatisch gestartet (Server / Client / Both)
- Post-Build-Aktionen:
  - Upload auf Itch.io bei `v-beta-client-itchio-*`
  - Vorbereitung für Steam bei `v-beta-client-steam-*`
  - Server vorbereiten oder live schalten (`prepare` / `live`)
- Deployment:
  - Upload/Distribution der Clients
  - Server-Restart/Neustart falls erforderlich
  - Automatische Ankündigungen (optional: Discord, Wartungsseite etc.)

# 🗂 Struktur der Repositories und Builds

```plaintext
Root
├── .github
│   └── workflows
│       ├── unity-build-client.yml
│       ├── unity-build-server.yml
│       ├── unity-build-both.yml
│       ├── unity-build-beta-client-itchio.yml
│       ├── unity-build-beta-client-steam.yml
│       ├── unity-build-beta-server-prepare.yml
│       ├── unity-build-beta-server-live.yml
│       └── unity-build-beta-both.yml
├── Builds
│   ├── Server
│   │   └── ServerBuild.exe
│   ├── Client
│   │   └── ClientBuild.exe
│   └── Logs
│       ├── client_build_log.txt
│       └── server_build_log.txt
```

# 📋 Tag-Übersicht und zugeordneter Workflow

| Tag Pattern | Ausgelöster Workflow | Besonderheit |
| --- | --- | --- |
| `v-dev-client-*` | `unity-build-client.yml` | Client lokal bauen |
| `v-dev-server-*` | `unity-build-server.yml` | Server lokal bauen |
| `v-dev-both-*` | `unity-build-both.yml` | Server und Client bauen |
| `v-beta-client-itchio-*` | `unity-build-beta-client-itchio.yml` | Upload zu Itch.io |
| `v-beta-client-steam-*` | `unity-build-beta-client-steam.yml` | Vorbereitung für Steam |
| `v-beta-server-prepare-*` | `unity-build-beta-server-prepare.yml` | Wartungsmodus starten |
| `v-beta-server-live-*` | `unity-build-beta-server-live.yml` | Server Liveschaltung |
| `v-beta-both-*` | `unity-build-beta-both.yml` | Voller Rollout |

# 🧠 Hinweise

- **Versionierung**: Tags wie `v-beta-client-itchio-1.0.0` werden zukünftig automatisch als Build-Version für Uploads genutzt.
- **Skalierbarkeit**: Weitere Plattformen (Testserver, Staging-Umgebungen) können einfach ergänzt werden.
- **Erweiterungen**: Discord/Webhook-Integration für Wartungsankündigungen geplant.

# 🚀 Vorteile

- Entwickler müssen lokal keine Builds mehr ausführen oder erstellen.
- Spart je nach Projektgröße pro Build zwischen 10 und 30 Minuten.
- Drastische Reduktion des manuellen Aufwands beim Release-Management (Ziel: unter 1 % der Entwicklungszeit).
- Schnellere Auslieferung von:
  - Releases
  - Hotfixes
  - Community-Testversionen
- Konsistente Rollout-Prozesse: Minimierung von Fehlern beim Deployment
- Automatisiertes Client-Update: Keine manuelle Nacharbeit für User
- Einfache Anbindung neuer Plattformen wie Steam oder eigene Launcher

# 🛠 Planung

| Sprint | Inhalt |
| --- | --- |
| Sprint 0 | Setup der Infrastruktur (Runner, Workflows, Umgebungsvariablen, Itch.io/Steam-Tools) |
| Sprint 1 | Erste Testdurchläufe: Build & Deployment-Pipeline verifizieren. Analyse, inwieweit Unity Unit Tests in die CI integriert werden können. |
| Sprint 2/3 | Produktiver Einsatz: Nutzung der Release Pipelines für neue Releases. Optional: Erweiterung durch automatisierte Tests nach erfolgreicher Validierung. |

---

