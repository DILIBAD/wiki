---
title: Game Design Dokument
description: 
published: true
date: 2025-04-19T19:07:37.917Z
tags: 
editor: markdown
dateCreated: 2025-04-17T21:46:46.966Z
---

# 🛠️ Game Design Dokument – DILIBAD

> **Spielname:**  
> **DILIBAD – Discover. Liberate. Build. Automate. Defend**  
>  
> **Setting:**  
> Fantasywelt mit mittelalterlicher Technologie, Magie als zentrales Element.  
> Erweiterungspotenzial: Arkan-Technologie, Steampunk, Sci-Fi über Lore und Magie.

## 📝 Einleitung

Alle essenziellen Aspekte des Spiels werden in dieser Übersicht strukturiert zusammengefasst.  
Die Abkürzung **DILIBAD** steht für die Kern-Gameplay-Bereiche und beschreibt den typischen Spielfortschritt im Rahmen der Spielmechanik.  
Jede Komponente basiert auf dem Fantasy-Setting und wird durch Magie, mittelalterliche Ressourcen und kooperative Spielmechaniken getragen.

## 1. 🎮 Überblick

**Spielname:**  
DILIBAD -  Discover. Liberate. Build. Automate. Defend

**Genre:**  
2.5D Top-Down | Coop | Multiplayer | Action RPG | Survival | Base-Building

**Plattform(en):**  
Windows First
- PC (Steam)  
- PC (itch.io)
- PC (Web Game)


**Multiplayer:**  
- Mirror-basiertes Online Spiel
- Koop (bis zu 4-8 Spieler) Kleine in sich geschlossene Runde ohne Save
- Dedicated Server Option für größere Gruppen und längere Runden

**Kurze Beschreibung:**  
Ein Kooperativem Spiel, bei dem Spieler gemeinsam Rohstoffe abbauen, eine Basis errichten, Wellen angreifender Gegner abwehren und sich auf einen  epischen Bosskampf vorbereiten.

---

## 2. 🔁 Core Gameplay Loop

**Haupt-Loop:**  
1. Rohstoffe abbauen  
2. Ressourcen zur Basis bringen  
3. Basis errichten/erweitern  
4. Angriffswelle nähert sich  
5. Verteidigung & Kampf  
6. Loot & Reparaturen  
7. Welt erweitern  
8. Hinweise zum Boss entdecken  
9. Bosskampf

---

## 3. 🕹️ Spielmodi

-  Koop Session  (bis zu 4-8 Spieler) 
- Living World (größere Gruppe mit Dedicated Server)
- Solo-Modus (optional)
- Schwierigkeitsgrade:  Für die Session/ Welt einstellbar ; Klassen haben unterschiedliche Komplexität und bieten somit auch eine Form der Schwierigkeit


---

## 4. ⚙️ Spielmechaniken

### Ressourcenabbau
- Ressourcenarten: Grundmaterialien, Besondere Materialien
- Werkzeuge & Upgrades
- Spawnsystem & Abbaugeschwindigkeit

### Basisbau
- Bauarten: 
- Upgrades & Reparatur
- Platzierung & Struktur-Grid

### Angriffswellen
- Wellenanzeige / Bedrohungslevel
- Gegnertypen: [Nahkampf, Fernkampf, Bosse]
- Angriffsmuster & Spawnpunkte

### Kampf
- Waffentypen: [Nahkampf, Fernkampf, Spezialwaffen]
- Ausrüstungsarten
- Fähigkeiten / Skillsystem

### Erkundung
- Karte & Bereiche
- Weltvergrößerung durch Abbau und Erkundung
- Dynamische Weltelemente / Events

### Hinweise zum Boss
- Sammelobjekte
- NPCs / Terminals
- Rätsel oder Wegweiser

### Bosskampf
- Standort / Arena
- Boss-Phasen & Fähigkeiten
- Voraussetzungen zum Start

---

## 5. 🗺️ Weltstruktur & Leveldesign

- Kartenstruktur: [Offene Welt / Modular / Zonenbasiert] TBD
- Biome / Umgebungen: [Wald, Höhlen, Labor, etc.] TBD
- Gatekeeping-Mechaniken: [Zugang nur mit bestimmten Tools / Fortschritt] TBD

---

## 6. 🎯 Progression & Belohnung

- Spielerlevel / Skillbaum
- Basis-Forschung / Upgrades
- Loot-Tiers 
- Freischaltungen: Waffen, Ausrüstung, Gebäude

---

## 7. 🧭 UI / UX Design

- HUD: Ressourcenanzeige, Wellen-Timer, HP
- Kampfbasierte Feedback Elemente für bevorstehende Angriffe
- Minimap / Weltkarte
- Inventar & Crafting/Produktions-Menü
- Bauinterface
- Koop-spezifische Anzeigen (z. B. Ping, Spielerstandorte)

---

## 8. 🌐 Multiplayer (Mirror)

- Netzwerkschema: Host-Client / Dedicated Server
- Synchronisation: Positionen, Ressourcen, Gegner, Bauten
- Koop-Features: Gemeinsamer Fortschritt, Heilung, Rollenverteilung

---

## 9. 🧪 Technische Details

- **Engine:** Unity (Version: Unity 6000.0.46f1  
- **Networking:** Mirror  
- **Persistenz:** Save/Load-System (lokal / Cloud?)  
- **Controller-Support:** Geplant sofern Portierung auf keine Probleme stößt

---

## 10. 🎨 Art Direction

- Perspektive: 2.5D Top-Down mit Tiefe
- Stil: 
- Charakterdesign
- Gegnerdesign
- Umwelt: Biome, Lichtstimmung, Partikeleffekte

---

## 11. 🔊 Sound Design

- Hintergrundmusik: Dynamisch je nach Situation
- Soundeffekte: Abbau, Kampf, UI, Basisbau
- Gegnergeräusche / Warnsysteme

---

## 12. 📖 Story & Lore

- Grundlegende Geschichte / Setting
- Ziel & Motivation der Spieler
- Ursprung der Gegnerwellen
- Hinweise auf den Boss

---

## 13. 💸 Monetarisierung

- Einmaliger Kauf  
- DLCs / Erweiterungen  
- Optionale Cosmetics?  
- Kein Pay-to-Win

---

## 14. 📆 Meilensteine & Roadmap

| Meilenstein | Inhalt                                 | Datum (geplant) |
| ----------- | -------------------------------------- | --------------- |
| MVP         | Grundlegender Loop, Coop, 1 Boss       | [20.05.2025]    |
| Alpha       | Erste Testphase mit Feeback-Loop       | [10.06.2025]    |
| Beta        | Content-Komplett, Bugfixing, Balancing | [01.07.2025]    |
| Launch      | Full Release/ Abgabe                   | [06.07.2025]    |

---

## 15. 📎 Anhang

- Skizzen & Konzeptgrafiken  
- Flowcharts (Gameplay, UI, Networking)  
- Asset-Liste  
- Referenzen

---
