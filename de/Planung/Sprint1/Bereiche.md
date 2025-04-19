---
title: Bereiche Sprint 1
description: 
published: true
date: 2025-04-19T11:57:46.799Z
tags: 
editor: markdown
dateCreated: 2025-04-19T11:55:31.233Z
---

# 🔷 Allgemeine Idee

- **Konstantin** übernimmt einen **Kernbereich mit hoher Systemtiefe** (z. B. Bereich **B: Core-Systeme & Technik**).
- Zusätzlich kümmert sich Konstantin um die **übergreifende Architektur**, z. B.:
  - Struktur der Services
  - Architekturempfehlungen
  - Aufbau des `ServiceLocator`
  - Ordnungsgemäße Trennung zwischen Zuständigkeiten

---

- Die anderen Teammitglieder übernehmen **Feature-orientierte Bereiche**, die klar abgegrenzt und über Schnittstellen angebunden sind.
- Jeder Bereich ist so gestaltet, dass er:
  - **modular erweiterbar**
  - **ohne direkte Abhängigkeiten**
  - und **mit klarer Zuständigkeit** umgesetzt werden kann.

---

## Vorteile dieser Struktur

- **Skalierbarkeit**: Neue Systeme können einfach integriert werden
- **Übersichtlichkeit**: Zuständigkeiten sind klar verteilt
- **Geringe Kopplung**: Systeme kommunizieren nur über definierte Services
- **Schnelle Integration**: Konstantin kann bei Bedarf in jedem Bereich unterstützen, ohne die Struktur zu brechen




# 🧩 Bereichsaufteilung mit Zuordnung & Vorschlag

| **Bereich** | **Fokus**                                                   | **Zuständig**      | **Anmerkung**                                                                 |
|-------------|-------------------------------------------------------------|--------------------|--------------------------------------------------------------------------------|
| **A: Ressourcen & Basis** | Ressourcen sammeln, Lager, Produktionen, Basis-Upgrades       | Teammitglied A     | Gut modularisierbar, viele UI-Interaktionen                                    |
| **B: Core-Systeme & Technik** | Services, Networking, Spawn, Bootstrapper                        | **Konstantin**      | Herz des Projekts, ServiceLocator, Mirror-Setup, Infrastruktur, Architekturbasis |
| **C: Entities & Verhalten** | Spielerlogik, NPCs, StateMachine, Interaktionen                 | Teammitglied B     | Zentrale Spiellogik, stark angebunden an Bereich B                            |
| **D: Präsentation** | UI, UX, Audio, FX, Environment                                 | Teammitglied C     | Kreativ-technischer Bereich, ideal für stetige Polishing-Zyklen               |
| **E: Kampfsystem** | Skills, Waffen, Muster, Schaden                                  | Teammitglied D     | Feature-lastig, kann gut mit C und D kombiniert werden                         |


![image.png](/image.png)