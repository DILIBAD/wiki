---
title: 000-Projekt
description: 
published: true
date: 2025-04-28T06:44:54.296Z
tags: 
editor: markdown
dateCreated: 2025-04-27T19:58:03.255Z
---

# Übersicht

Entwickelt wird ein Spiel, das auf einem Multiplayer-First-Ansatz basiert und auf Unity 6 umgesetzt wird. Ziel ist es, ein stabiles, testbares Produkt zu entwickeln und parallel dazu eine engagierte Community aufzubauen, die aktiv in den Entwicklungs- und Testprozess eingebunden wird.

## Technologie
- **Engine:** Unity 6
- **Versionskontrolle:** GitHub – ermöglicht externen Testern, über Issues und Incident-Reports Feedback zu geben.

## Zeitplan Community & Social Media

- **Sprint 1:** Aufbau von Social-Media-Kanälen und Launch einer "Coming Soon"-Webseite ohne direkte Verlinkung auf den Discord-Server.
- **Sprint 2:** Aktivierung der Webseite mit minimalen Informationen über das Spiel sowie Integration eines Links zum Discord-Server nach Fertigstellung einer testbaren Version.
- **Sprint 3:** Vollständiger und produktiver Einsatz aller Kommunikationsplattformen, um maximale Reichweite und Engagement zu erzielen.

## Community-Strategie

Neben der Spieleentwicklung soll eine Community rund um das Spiel entstehen. Die Pflege und der Ausbau der Community sollen maximal 10 % des gesamten Projektaufwands betragen. Die Community wird aktiv in die Teststrategie eingebunden, um Qualität und Stabilität des Spiels kontinuierlich zu verbessern.

**Ablauf:**
- Interessenten werden von Social Media auf die zentrale Webseite weitergeleitet, auf der alle wichtigen Informationen über das Spiel bereitgestellt werden.
- Hauptkommunikationskanal wird ein Discord-Server sein, der den bidirektionalen Austausch mit der Community ermöglicht.
- Über einen eigens entwickelten Discord-Bot können Nutzer direkt aus Discord heraus Probleme und Vorschläge melden. Diese werden automatisiert über eine API an GitHub übertragen, um den Aufwand bei der Feedback-Sammlung zu minimieren.

## Social Media-Strategie

**[Platzhalter für detaillierte Ausarbeitung der Social Media-Strategie]**

## Testing-Strategie

Ziel ist es, das Spiel nicht nur intern, sondern auch durch Tests im Freundeskreis sowie durch externe Tester zu verbessern.
- Feedback wird zentral im öffentlichen GitHub-Repository im Diskussionsbereich gesammelt.
- Probleme und Bugs werden über die Issue-Funktion erfasst und nachverfolgt.

## Plattformen

- **Social Media:** **[Platzhalter für genaue Kanäle wie Twitter, Instagram, TikTok, etc.]**
- **Webseite:** [https://dilibad.de](https://dilibad.de)
- **Discord-Server:** **[Platzhalter für Invite-Link]**
- **GitHub Public Repository:** **[Platzhalter für Repo-Link]**
- **Distributionsplattformen:** Itch.io & Steam

## CI/CD-Strategie

- **Automatisierung durch Tags**: Push von spezifischen Tags (`v-dev-*`, `v-beta-*`) löst gezielte Workflows aus.
- **Workflows**: Server-, Client- oder kombinierte Builds, inkl. Verteilung (Itch.io, Steam, Liveserver).
- **Post-Build Aktionen**: Uploads, Wartungsmodus, Liveschaltung, optionale Discord/Webhook-Benachrichtigungen.

## 🛠 Build- und Deployment-Schritte

1. Tag pushen auf `dev` oder `main`.
2. Runner erkennt das Tag-Format.
3. Passender Build-Workflow wird automatisch gestartet.
4. Automatisches Deployment je nach Bedarf (lokal, Itch.io, Steam, Liveserver).

## 🗂 Struktur

- **Workflows**: `.github/workflows/`
- **Builds und Logs**: `Builds/`

## 📋 Tag-Übersicht

| Tag Pattern | Workflow | Besonderheit |
|:---|:---|:---|
| `v-dev-client-*` | Client lokal bauen | |
| `v-dev-server-*` | Server lokal bauen | |
| `v-dev-both-*` | Server und Client bauen | |
| `v-beta-client-itchio-*` | Client bauen + Upload Itch.io | |
| `v-beta-client-steam-*` | Vorbereitung Steam Upload | |
| `v-beta-server-prepare-*` | Wartungsmodus aktivieren | |
| `v-beta-server-live-*` | Liveschaltung Server | |
| `v-beta-both-*` | Komplett-Rollout (Server + Client) | |

## 🚀 Vorteile

- Kein lokales Bauen nötig → spart 10–30 Minuten pro Build.
- Einheitlicher und fehlerfreier Release-Prozess.
- Schnelle Auslieferung von Releases, Hotfixes und Testversionen.
- Skalierbar für weitere Plattformen.

## 🛠 Geplante Schritte

| Sprint | Inhalt |
|:---|:---|
| Sprint 0 | Infrastruktur-Setup (Runner, Workflows, Tools) |
| Sprint 1 | Testläufe & Validierung |
| Sprint 2/3 | Produktiver Einsatz + optionale Testautomatisierung |

[Details zum CI/CD Workflow](#)

## Git-Strategie (GitFlow)

**[Platzhalter für konkrete GitFlow-Beschreibung]**
