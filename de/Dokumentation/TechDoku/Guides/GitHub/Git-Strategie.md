# Git-Strategie (GitFlow)
Featues Based Git Flow 
## 🔀 Branch-Struktur

- `main`: Stabile Releases
- `dev`: Aktueller Entwicklungsstand
- `version/x.y.z`: Release-Vorbereitung
- `feature/...`: Feature-Entwicklung
- `test/beta`: Interne Tests
- `hotfix/x.y.z`: Dringende Bugfixes

## 📛 Branch-Namen

| Typ       | Format                               | Beispiel                |
|-----------|--------------------------------------|--------------------------|
| Version   | `version/x.y.z`                      | `version/1.2.0`          |
| Feature   | `feature/<ticket>/<kurzbeschreibung>` | `feature/42/user-login` |
| Hotfix    | `hotfix/x.y.z`                       | `hotfix/1.2.1`           |
| Test/Beta | `test/beta`                          | `test/beta`              |

## 🔁 Workflow

1. **Feature-Branch** aus `version/x.y.z`
2. **PR**: `feature/...` → `version/x.y.z` (1 Review)
3. **Merge**: `version/x.y.z` → `dev` (2 Reviews, Tag für Testbuilds)
4. **Merge**: `dev` → `main` (Release, Tag, Changelog)
5. **Hotfix**: `hotfix/x.y.z` von `main`, zurück auf `main` + `dev`

## ✅ Pull-Request Regeln

| Merge            | Reviewer      |
|------------------|----------------|
| `feature → version` | 1 Reviewer    |
| `version → dev`   | 2 Reviewer     |
| `dev → main`      | 6 Reviewer     |
| `hotfix`          | 1–2 Reviewer   |

## 📈 Performance-Regeln

- Max. 2 offene Reviews pro Person
- Reviews schnell abarbeiten, um Merge-Konflikte zu vermeiden
- CI-Checks:  Build, Tests (Sprint 2)
- PR-Labels: `ready-for-review`, `needs-changes`, `done`