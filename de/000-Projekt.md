---
title: 000-Projekt
description: 
published: true
date: 2025-04-28T11:58:06.711Z
tags: 
editor: markdown
dateCreated: 2025-04-27T19:58:03.255Z
---

# Übersicht

Entwickelt wird ein Spiel, das auf einem Multiplayer-First-Ansatz basiert und auf Unity 6 umgesetzt wird. Ziel ist es, ein stabiles, testbares Produkt zu entwickeln und parallel dazu eine engagierte Community aufzubauen, die aktiv in den Entwicklungs- und Testprozess eingebunden wird.

[Game Design Dokument für Sprint 1](/de/001-GDD/Sprint1/GDD-Sprint1)


## Technologie
- **Engine:** Unity 6
- **Versionskontrolle:** GitHub – ermöglicht externen Testern, über Issues und Incident-Reports Feedback zu geben.

# Technische Architektur - Unity Bootstrapper & Service-Management

## Übersicht

Zur zentralen Initialisierung des Spiels wird ein **Bootstrapper** in Unity eingesetzt. Dieser ist dafür verantwortlich, das Spielsystem geordnet zu starten und notwendige Services bereitzustellen.

## Service Management

Nach dem Start des Bootstrappers werden verschiedene **Services** in einen **Service Manager** eingebunden. Diese Services werden über einen **globalen Service Locator** zur Verfügung gestellt. Dadurch können Systeme in der Game-Mechanik einfach auf notwendige Services zugreifen, ohne direkte Referenzen zueinander zu benötigen.

Ziel dieses Ansatzes ist es:
- **Kopplungen zwischen Systemen zu vermeiden**,
- die **Kommunikation über zentrale Services** zu steuern,
- und damit eine **robuste, wartbare Architektur** sicherzustellen, die **"Spaghetti-Code"** weitestgehend verhindert.

## Interface-basierte Entwicklung

Um eine möglichst **unabhängige und effiziente** Entwicklung zu gewährleisten, wird für jede Service-Kommunikation ein eigenes **Interface** definiert. Die Implementierung erfolgt **gegen diese Interfaces** und nicht direkt gegen konkrete Klassen. 

Vorteile dieser Strategie:
- Services können **einfach ausgetauscht** werden, ohne große Änderungen in der Codebasis vorzunehmen.
- Abhängigkeiten werden **nur in Tests** relevant, nicht bei der initialen Implementierung.
- Eine klare Trennung von Schnittstellen und Implementierungen sorgt für **höhere Flexibilität** und **bessere Testbarkeit**.

Änderungen an Interfaces müssen **zentral über einen definierten Prozess** erfolgen:  
👉 [Link zum Änderungsprozess](#)

## Validierung im Sprint 1

Diese Architektur wird im **Sprint 1** eingeführt, verifiziert und getestet.  
Sollten im Rahmen der Implementierung Probleme auftreten, wird gezielt darauf eingegangen und gemeinsam an **lösungsorientierten Verbesserungen** gearbeitet.


> **Langfristiges Ziel:**  
Durch den konsequenten Einsatz von Interfaces und Service-basierten Kommunikationsstrukturen soll die Codebasis **nachhaltig erweiterbar und wartbar** bleiben, bei gleichzeitig minimalem Aufwand für spätere Anpassungen.

---
# Zeitplan Community & Social Media

- **Sprint 1:** Aufbau von Social-Media-Kanälen und Launch einer "Coming Soon"-Webseite ohne direkte Verlinkung auf den Discord-Server.
- **Sprint 2:** Aktivierung der Webseite mit minimalen Informationen über das Spiel sowie Integration eines Links zum Discord-Server nach Fertigstellung einer testbaren Version.
- **Sprint 3:** Vollständiger und produktiver Einsatz aller Kommunikationsplattformen, um maximale Reichweite und Engagement zu erzielen.

# Community-Strategie

Neben der Spieleentwicklung soll eine Community rund um das Spiel entstehen. Die Pflege und der Ausbau der Community sollen maximal 10 % des gesamten Projektaufwands betragen. Die Community wird aktiv in die Teststrategie eingebunden, um Qualität und Stabilität des Spiels kontinuierlich zu verbessern.

**Ablauf:**
- Interessenten werden von Social Media auf die zentrale Webseite weitergeleitet, auf der alle wichtigen Informationen über das Spiel bereitgestellt werden.
- Hauptkommunikationskanal wird ein Discord-Server sein, der den bidirektionalen Austausch mit der Community ermöglicht.
- Über einen eigens entwickelten Discord-Bot können Nutzer direkt aus Discord heraus Probleme und Vorschläge melden. Diese werden automatisiert über eine API an GitHub übertragen, um den Aufwand bei der Feedback-Sammlung zu minimieren.

# Social Media-Strategie

Aufgrund der Ergebnisse der Zielgruppenanalyse und dem Betrachten des aktuellen Zeitgeistes wurde die Entscheidung getroffen, das Projekt „DILIBAD“ und seine Entstehung auf den Social-Media-Plattformen „Twitter“, „Instagram“ und „Discord“ zu begleiten.

## Zielsetzung
Das Ziel der Social-Media-Präsenz ist der Aufbau einer Community für das Testen und Spielen des Spiels.

## Zielgruppe
Durch die Schnittmenge mehrerer Statistiken zum Thema „Social Media Nutzung“ und „Gaming“ hatte sich herauskristallisiert, dass unsere Hauptzielgruppe in der Altersgruppe 14–29 liegt. Die Gruppe der 30- bis 49-Jährigen stellte sich ebenfalls als valide heraus, obwohl die Social-Media-Nutzung der Gruppe abnimmt [1] [2].

## Plattformen

- **Social Media:** **[Platzhalter für genaue Kanäle wie Twitter, Instagram, TikTok, etc.]**
- **Webseite:** [https://dilibad.de](https://dilibad.de)
- **Discord-Server:** **[Platzhalter für Invite-Link]**
- **GitHub Public Repository:** **[Platzhalter für Repo-Link]**
- **Distributionsplattformen:** Itch.io & Steam

Bei der Wahl der vorerst zu bespielenden Plattformen hat man sich aufgrund der Beliebtheit bei der Zielgruppe für Instagram, Twitter und Discord entschieden.
Instagram wurde aufgrund der Beliebtheit bei der Zielgruppe ausgewählt [1], während Twitter für eine schnelle Kommunikation mit der Zielgruppe gewählt wurde.
Des Weiteren hat sich aus der Recherche ergeben, dass Discord nicht nur der fünftbeliebteste Messenger in Deutschland ist, sondern vor allem in der Zielgruppe der Gamer besonders häufig genutzt wird [3].
Basierend auf diesen Ergebnissen hat man sich deshalb dafür entschieden, einen öffentlichen Discord-Server zu betreiben, auf dem potenzielle Tester und Liebhaber des Spiels miteinander kommunizieren können.

## Content-Strategie
Für die Plattform Instagram wurden für den Anfang die Formate „Shared Screen“, „Shared Code“ und „DEVS IRL“ ausgearbeitet.
In „Shared Screen“ werden Bildschirmaufnahmen der Entwickler geteilt, die besonders auffällig sind und somit interessant für die Abonnenten sein könnten.
In „Shared Code“ werden wiederum Bildschirmaufnahmen des Programmiercodes geteilt, die die Entwickler zu besonderen Erkenntnissen geführt haben.
„DEVS IRL“ soll das Team vorstellen.
Für die Plattform Twitter fiel die Wahl vorläufig auf die Formate „Gedanken beim Entwickeln“, „Schwierigkeiten beim Entwickeln“, bei denen die Namen für ihren Inhalt stehen.
Für die Ansprache auf den Plattformen hat man sich für einen informellen und zwangslosen Ton und die Du-Ansprache entschieden, um eine Nähe zum Publikum aufzubauen.
Die Wahl der Plattformen und der Formatideen wird im Laufe des Projektes validiert, verändert und/oder ersetzt.

## Redaktionsplan
Der Redaktionsplan wird in Sprint 1 entwickelt und an die zeitlichen Ressourcen des Teams angepasst. Eventuell wird eine Software wie „Buffer“, „Later“ oder „Trello“ genutzt, um die Veröffentlichung des Contents zeitlich optimal zu gestalten und den Redaktionsaufwand somit zu verringern.

## Krisenkommunikation
Grundlegend wurde festgelegt, dass mit Fehlern offen umgegangen werden soll und die Community mit Ehrlichkeit behandelt wird.

## Rollen und Verantwortlichkeiten
Für die Bespielung der Instagram- und Twitter-Kanäle inklusive Community Management ist hauptsächlich Dmytro Skorbyashchenskyy verantwortlich.
Das Community Management auf Discord übernehmen Pascal Meier und Alexander Mutig.
Alle Fotos werden von Pascal Meier erstellt.



# Testing-Strategie

Ziel ist es, das Spiel nicht nur intern, sondern auch durch Tests im Freundeskreis sowie durch externe Tester zu verbessern.
- Feedback wird zentral im öffentlichen GitHub-Repository im Diskussionsbereich gesammelt.
- Probleme und Bugs werden über die Issue-Funktion erfasst und nachverfolgt.

# CI/CD-Strategie

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

[Details zum CI/CD Workflow](/de/004-Workflows/VersionControl-Release)

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
[Details zum GIT Workflow](/de/004-Workflows/VersionControl-Release)