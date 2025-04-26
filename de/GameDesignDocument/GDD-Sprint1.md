---
title: Game Design Dokument - Sprint 1
description: 
published: true
date: 2025-04-26T16:18:07.266Z
tags: 
editor: markdown
dateCreated: 2025-04-20T12:06:35.065Z
---

# Disclaimer
> Dieses Dokument konzentriert sich ausschließlich auf die Gameplay-Mechaniken, die für den ersten Sprint benötigt werden. Erweiterungen werden separat dokumentiert und bei Bedarf in spätere Versionen des Game Design Documents (GDD) integriert.  
> Ziel: Klare, übersichtliche Dokumentation – damit alle Entwickler nur die relevanten Informationen für den aktuellen Sprint aufnehmen und umsetzen können.

---

# 🎮 Spielübersicht

## 📝 Steckbrief

| Kategorie         | Details |
| :---------------- | :------ |
| **Genre**         | Kooperatives Action-RPG mit Base-Building und Tower Defense-Elementen |
| **Plattformen**   | Windows (primär), WebGL (optional) |
| **Spieleranzahl** | 4 Spieler (kooperativ) |
| **Perspektive**   | 2D / 2.5D Top-Down (8-Richtungs-Bewegung) |
| **Technologie**   | Unity 6, GitHub, Wiki (für Doku), Discord & WhatsApp (Kommunikation) |

## 🎯 Gameplay-Säulen
- **Discover** – Erkunden
- **Build** – Basis bauen & aufwerten
- **Defend** – Basis verteidigen  
_(Hinweis: Liberate & Automate sind optionale Erweiterungen für spätere Versionen.)_

## 🔀 Elevator Pitch

**DILIBAD** ist ein kooperatives Action-RPG für bis zu 4 Spieler, das Erkundung, Basisbau und Verteidigung zu einem dynamischen Multiplayer-Erlebnis verbindet.  
Sammelt Ressourcen, errichtet eine automatisierte Basis und trotzt eskalierenden Feindwellen, um ein Portal zu öffnen und den finalen Boss zu besiegen.  
**Discover. Build. Defend. – DILIBAD: Erobern beginnt mit Zusammenarbeit.**

---

# 🔄 Gameloop

## 1. Ressourcen sammeln & Aufrüstung
- Kristalle abbauen und für Charakter- und Basis-Upgrades nutzen.
- Monster werden auf die Sammelaktivitäten aufmerksam.

## 2. Gefahr eskaliert
- Fortlaufendes Sammeln/Kämpfen erhöht die Bedrohungsanzeige.
- Bei 100 % Aufmerksamkeit startet eine große Angriffswelle des Bosses.

## 3. Basisverteidigung
- Verteidigt die Basis und den Kern gegen angreifende Wellen.
- Nutzt Kristalle für Upgrades:
  - Charakterverbesserungen (Angriff, Verteidigung)
  - Basisverteidigungsanlagen (primitive Türme, Strukturen)

## 4. Portal & Bosskampf
- Findet 3 **Schlüsselkristalle**.
- Aktiviert das **Portal**, um den Bosskampf auf einer separaten Karte zu starten.

## 5. Sterben & Respawn
- Sterbende Spieler respawnen in der Basis.
- Beim Scheitern im Bosskampf:
  - Option A: Game Over
  - Option B: Respawn + verstärkte Angriffswelle (zweite Chance)

## 6. Spielende
- **Sieg:** Boss besiegt
- **Niederlage:** Basiskern zerstört oder alle Spieler dauerhaft tot

---

# 🛠️ Feature- und Systemübersicht

## 🧟‍♂️ Spielermechaniken
- Charakterbewegung (8 Richtungen)
- Kristalle sammeln und Inventar verwalten
- Upgradesystem für:
  - Angriff
  - Verteidigung
  - Klassenfertigkeiten
- Respawn-Mechanik (Rückkehr zur Basis nach Tod)

## 🧑‍ Spielerklassen (Sprint 1)

| Klasse    | Rolle | Upgradeoption |
|:----------|:------|:--------------|
| **Sammler** | Fokus auf Ressourcensammeln | Schnelleres Abbauen |
| **Kämpfer** | Fokus auf Kampf | Höhere Angriffsgeschwindigkeit |

---

## 👾 Gegner- und Gefahrensysteme
- Grundmonster, die Spieler und Basis angreifen
- Gegnerwellen basierend auf Bedrohungslevel
- Aufmerksamkeitssystem (Bedrohungsanzeige)
- Bosswelle bei voller Aufmerksamkeit

---

## 🏰 Basis- und Verteidigungssysteme
- **Spielerbasis**: Rückzugsort und Verteidigungspunkt
- **Basiskern**: Ziel der Gegner – Verlust führt zu Game Over
- **Verteidigungsmöglichkeiten**:
  - Platzierbare primitive Tower (Single Target)
  - Aufwertbare Strukturen
  - Bau neuer Strukturen auf freien Slots

---

## 🔮 Fortschrittssystem: Portal & Boss
- Portal: Spezialstruktur zum Aktivieren des Bosskampfes
- 3 Schüsselkristalle erforderlich
- Separate Karte für den Bosskampf nach Portalöffnung

---

## 🧬 Spielregeln & Zustände
- **Sieg:** Boss besiegt
- **Niederlage:** Basiskern zerstört oder kompletter Ausfall beim Bosskampf
- **Zustände**:
  - Erkundung & Ressourcengewinnung
  - Aktive Angriffswellen
  - Portalaktivierung
  - Bosskampf

---

## 🌍 Weltstruktur
- Interaktive **Kristallquellen**
- **Gegnerspawnpunkte** über die Karte verteilt
- **Basis**, **Portal** und **Kristallfelder** als Schlüsselorte
- Separate Bosskarte

---

## 🧽 Benutzeroberfläche & Feedbackelemente
- Ressourcenanzeige (Kristalle)
- Upgradebuttons mit Status
- Bedrohungsanzeige (Balken oder Prozent)
- Anzeige gesammelter Schüsselkristalle
- Sieges- und Game Over-Screen