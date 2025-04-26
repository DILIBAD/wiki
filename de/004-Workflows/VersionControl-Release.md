---
title: Version Control & CI/CD
description: 
published: true
date: 2025-04-20T09:07:48.268Z
tags: 
editor: markdown
dateCreated: 2025-04-20T09:07:48.268Z
---

# 🚀 Git Flow & CI/CD

In diesem Abschnitt wird der Git-Workflow für das DILIBAD-Projekt sowie die zugehörige CI/CD-Infrastruktur beschrieben. Ziel ist es, eine klar strukturierte Entwicklungsumgebung mit effizientem Release-Management zu schaffen, die langfristig Zeit spart, Fehler reduziert und ein schnelles Feedback ermöglicht – insbesondere in Kombination mit dem geplanten Community-Building.

---

## ⚙️ CI/CD

Die automatisierte Build- und Deployment-Pipeline soll zentrale Prozesse wie Release-Management, Testing und Updates vereinfachen.

### Ziele:

- **Automatischer Build** bei bestimmten Tags in `dev` oder `main`
- **Deployment** der jeweiligen Version (Server & Clients)
- **Automatische Ankündigung** eines Wartungsfensters (15 Minuten)
- **Shutdown & Rollout** der neuen Server- und Client-Versionen
- **Automatisiertes Client-Update** ohne manuelle Eingriffe

### Planung:

- **Sprint 0**: Setup & Infrastruktur vorbereiten  
- **Sprint 1**: Erste Tests & Validierung der Abläufe  
- **Sprint 2/3**: Nutzung der Release Pipeline für ausrollen neuer Versionen. Optionale Erweiterung durch Unity Unit Tests (noch zu prüfen)

### Vorteile:

Nach initial hohem Implementierungsaufwand reduziert sich der manuelle Aufwand langfristig deutlich (Ziel: unter **1 %** der Entwicklungszeit). Dies ermöglicht eine schnellere Auslieferung von Releases, Hotfixes und Testversionen – besonders im Zusammenspiel mit der aktiven Community.

> **Hinweis:** Die CI/CD-Pipeline ist ein integraler Bestandteil der Community-Strategie. Ein Scheitern dieser Infrastruktur erfordert eine entsprechende Anpassung des Community-Workflows.

---

## ⚠️ Risiken

- **Hoher initialer Aufwand** für Aufbau der Infrastruktur
- **Technische Komplexität** kann zu unerwarteten Blockern führen
- **Fehlende Tools** oder Inkompatibilität mit Unity-Umgebung möglich

### Maßnahme:

- Nutzen klar überwiegt langfristig den Aufwand
- Falls unlösbare technische Probleme auftreten, muss der Community-Aufbau entsprechend reduziert oder neu geplant werden

---

## 🧠 Git Workflow (für 6 Personen)

Ein strukturierter Git-Workflow regelt die Zusammenarbeit im Team und stellt sicher, dass alle Änderungen nachvollziehbar, geprüft und konfliktfrei eingebunden werden.

---


# 🧠 Git-Workflow (6 Personen)

## 🔀 Branch-Struktur

```text
main            → Releases (stable)
dev             → Aktueller Entwicklungsstand
version/x.y.z   → Vorbereitung einer konkreten Version
feature/...     → Einzelne Features
test/beta       → Interne Tests / Beta-Testing
hotfix/x.y.z    → Bugfix nach Release
```

---

## 📛 Namenskonventionen

| Branch-Typ | Format                            | Beispiel                    |
|------------|-----------------------------------|-----------------------------|
| Version    | `version/x.y.z`                   | `version/1.2.0`             |
| Feature    | `feature/<ticket>/<kurzbeschreibung>` | `feature/42/user-login`    |
| Hotfix     | `hotfix/x.y.z`                    | `hotfix/1.2.1`              |
| Test/Beta  | `test/beta`                       | `test/beta`                 |

---

## 🔁 Workflow-Ablauf

1. Feature beginnt auf:  
   `version/x.y.z → feature/...`

2. PRs von Feature zurück in `version/x.y.z`  
   → 1 Review erforderlich

3. Merge `version/x.y.z` → `dev`  
   → 2 Reviews (inkl. Autor:in)

4. Merge `dev` → `main` (Release)  
   → Tag setzen (`v1.2.0`), Changelog updaten

5. Hotfix (dringend)  
   → Start auf `main` → `hotfix/x.y.z` → zurück in `main` und `dev`

---

## ✅ Pull Request Regeln

| Ziel-Branch     | Reviewer | Regel                          |
|------------------|----------|-------------------------------|
| `feature/*`      | 1        | Pflicht-Review (leichtgewichtig) |
| `version/* → dev`| 2        | Qualitätssicherung             |
| `dev → main`     | 6        | Finaler Release                |
| `hotfix/*`       | 1–2      | Je nach Schwere des Bugs       |

---

## 📈 Performance-Regeln

- Max. 2 offene Reviews pro Person gleichzeitig, reviews sind zeitnah zu erledigen damit merge conflicts verhindert werden
- CI: Linting, Build, Tests
- Labels: `ready-for-review`, `needs-changes`, `done`

