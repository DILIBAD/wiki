---
title: GitHub Workflow
description: 
published: true
date: 2025-05-08T11:46:42.246Z
tags: 
editor: markdown
dateCreated: 2025-04-20T09:07:48.268Z
---

# 🧠 Git-Workflow
Informationen wann Rebase und wann Squash, derzeit noch nicht beschlossen ob dies genutzt werden soll:
[Rebase vs merge vs squash](https://medium.com/@shikha.ritu17/git-rebase-vs-merge-vs-squash-choosing-the-right-strategy-for-version-control-a9c9bb97040e)

Um eine gute Versionierung zu erlauben werden wir uns an das Semantik Versioning halten.
TLDR:
0.0.0
MajorChange.NewFeatures.Patch

Major Change meint das es Änderungen gibt die dafür sorgen das es keine Backwards Kompatibilität gibt. Das heißt bspw. Interface Änderungen von Interfaces die bereits definiert wurden und aktiv genutzt werden.

New Feature meint einfach das es neue Funktionalitäten gibt

Patch enthält nur Änderungen an bereits bestehenden Systemen, bspw. das Fixen eines bugs oder veränderung der Funktion.
[Semantik Version](https://www.youtube.com/watch?v=rTZ35Subk9U)
## 🔀 Branch-Struktur

```text
main            → Releases (stable)
dev             → Aktueller Entwicklungsstand
version/x.y.z   → Vorbereitung einer konkreten Version
feature/...     → Einzelne Features
import/...      → Importieren von Assets IMMER ungebunden von features
test/beta       → Interne Tests / Beta-Testing
hotfix/x.y.z    → Bugfix nach Release
```

---

## 📛 Namenskonventionen

| Branch-Typ | Format                            | Beispiel                    |
|------------|-----------------------------------|-----------------------------|
| Version    | `version/x.y.z`                   | `version/1.2.0`             |
| Feature    | `feature/<ticket>/<kurzbeschreibung>` | `feature/42/user-login`    |
| Feature    | `import/<assetbeschreibung>` | `import/playerassets`    |
| Hotfix     | `hotfix/x.y.z`                    | `hotfix/1.2.1`              |
| Test/Beta  | `test/beta`                       | `test/beta`                 |

---

## 🔁 Workflow-Ablauf

1. Feature beginnt auf:  
   `version/x.y.z → feature/...`

2. PRs von Feature zurück in `version/x.y.z`  
   → 1 Review erforderlich

3. Merge `version/x.y.z` → `dev`  
   → 2 Reviews (inkl. Autor:in) Tag setzten für test builds wenn merge vollständig ist

4. Merge `dev` → `main` (Release)  
   → Tag setzen , Changelog updaten

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

