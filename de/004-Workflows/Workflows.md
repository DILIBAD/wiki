---
title: Workflows
description: 
published: true
date: 2025-04-23T17:54:36.514Z
tags: 
editor: markdown
dateCreated: 2025-04-20T08:18:13.787Z
---

# Workflows

In diesem Dokument werden alle Workflows in einem TLDR Format zusammengefasst. Ausführliche Fasungen sind zum jeweiligen Beitrag verlinkt.
Durch das festhalten der Workflows ist der Prozess klar nachzulesen und ermöglicht eine flexible Einarbeitung ohne große Absprache.


## Übersicht

### 🧠 Community Workflow – TL;DR

Der Community-Workflow dient dazu, frühzeitig eine engagierte Spiel-Community aufzubauen, um Feedback, Ideen und Bugs zu sammeln und so die Qualität von **DILIBAD** nachhaltig zu verbessern. Dabei kommen Discord, GitHub, eine zentrale Webseite sowie Social-Media-Kanäle zum Einsatz.

Ein Bot automatisiert Feedback-Prozesse und leitet Eingaben direkt weiter. Die Plattformpflege ist auf ein Minimum reduziert, um Entwicklungsressourcen zu schonen (max. 10 % Aufwand). Risiken wie Burnout, Scope Creep oder technischer Overhead werden durch klare Exit-Strategien und ständige Evaluation adressiert.

👉 [Zum vollständigen Workflow-Dokument](./Workflows/Community)



### ⚙️ Git Flow & CI/CD – TL;DR

Ein strukturierter Git-Workflow und eine automatisierte CI/CD-Pipeline sorgen für saubere Zusammenarbeit im Team und schnelle, zuverlässige Releases.

- **Git-Flow** mit Branch-Typen für `feature`, `version`, `hotfix` & `test`
- **Klare PR-Regeln** mit abgestufter Review-Anzahl je nach Ziel-Branch
- **Automatisierte CI/CD-Pipeline** für Builds, Updates & Deployment
- **Wartungsfenster & automatischer Server-Rollout** bei neuen Versionen
- **Langfristiges Ziel**: < 1 % Aufwand für Release-Management
- **Risikoabschätzung** & Rückfallstrategie bei technischen Blockern

👉 [Zum vollständigen Workflow-Dokument](./Workflows/VersionControl-Release)


### 📝 TL;DR – Verifizierungsprozess & Tagging (Kurzfassung)

- **Keine direkte Wiki-Verifizierung möglich** → Diskussion & Abstimmung erfolgen **per Kommentar**.
- **Tags zeigen Status von Seiten** – alle Teammitglieder pflegen diese aktiv.
- Neue Seiten starten **ohne Tag**, nach Sprint 0 erhalten sie `verifiziert`.

### 🔖 Wichtige Tags

| Tag                      | Bedeutung                                                |
|--------------------------|----------------------------------------------------------|
| `verifiziert`           | Inhalt ist aktuell, abgestimmt, verbindlich              |
| `in_bearbeitung`        | Seite wird gerade aktiv überarbeitet                     |
| `diskussion_bedarf`     | Änderungsvorschläge liegen vor, Entscheidung steht aus   |
| `verifizierung_ausstehend` | Änderung fertig, aber noch nicht reviewed/verifiziert |
| `ueberarbeitung_noetig` | Veraltet/unvollständig, Überarbeitung nötig              |

➡️ Änderungen = **Kommentar + passender Tag**  
➡️ Eskalationen = Weitere Person einbeziehen → ggf. ins Meeting bringen  
➡️ **Ziel:** Klarer Überblick & effiziente Kommunikation im Team

👉 [Zum vollständigen Workflow-Dokument](./Workflows/Dokumentation)