---
title: Sprint 1
description: 
published: true
date: 2025-04-19T00:32:33.999Z
tags: 
editor: markdown
dateCreated: 2025-04-18T23:14:28.702Z
---

# Sprint 1 – Ziel & Feature-Übersicht

## Zielsetzung
Bis Ende von **Sprint 1** soll eine einfache, aber funktionale **Multiplayer-Spielschleife** stehen. Fokus liegt auf:
- Kampf
- Basisbau
- Angriffswellen
- Ressourcenmanagement
- Grundlegender UI/UX
- Multiplayer mit Mirror

---

## 1. Spielstruktur

> **TL;DR:** Einstieg ins Spiel über Lore-Intro, Menü und Tutorial – danach Start einer Multiplayer-Session.

### Einstieg & Menüs
- Lore-Intro (Bilder + Audio)
- Hauptmenü
- Tutorial (Grundmechaniken)
- Start einer Game-Session (Multiplayer-fähig)

---

## 2. Gameplay-Systeme

> **TL;DR:** Spieler können kämpfen, sammeln, bauen und die Basis ausbauen – Angriffswellen reagieren auf Spieleraktionen.

### 2.1 Spielerfunktionen
- Klassen: Tank, DPS, Healer, Ranged
- Aktionen: Skills casten, kämpfen, looten, sammeln, interagieren
- Respawn in der Basis nach Tod

### 2.2 Basis & Fortschritt
- Zentrale Basis mit Lager, Upgrades und Verteidigung
- Herstellung von Ausrüstung und Gebäuden

### 2.3 Angriffswellen
- Wellen starten **durch Spieleraktionen**, nicht Timer
- Gegner kommen aus mehreren Richtungen
- Ziele: Basis & Umgebung
- Steigende Schwierigkeit

### 2.4 Kampfsystem
- Skill-Casting mit Zielindikator
- Gegnerangriffe visuell erkennbar & ausweichbar

---

## 3. NPCs & Gegner

> **TL;DR:** Einheitliches NPC-System für Freunde & Feinde – Boss-Gegner bringt spielerische Herausforderung.

### Gemeinsames System
- Alle NPCs teilen sich StateMachine & Sensorik
- Konfigurierbare Werte & Angriffsmuster

### Freundliche NPCs
- Unterstützen die Basis
- Erste einfache Iteration

### Feindliche NPCs
- Für Angriffswellen
- Einfaches Kampfverhalten

### Boss-Gegner
- Komplexe Angriffe
- Siegziel der Session

---

## 4. Benutzeroberfläche (UI)

> **TL;DR:** Spieler bekommt alle Infos zu Zustand, Kampf, Ressourcen und Zielerreichung übersichtlich dargestellt.

### Spielerinformationen
- Leben, Cooldowns, Skills
- Angriffswellen-Indikator
- Schaden & Respawn

### Gegnerinformationen
- Lebensanzeige

### Ressourcen & Basis
- Sichtbare Lagerbestände & Materialanforderungen

### Spielzustand
- Klar erkennbare Sieg/Niederlage-Anzeigen

---

## 5. Benutzererlebnis (UX)

> **TL;DR:** Sichtbare Rückmeldung bei Interaktionen & blockierten Aktionen für klare Spielbarkeit.

- Feedback für mögliche Aktionen
- Hinweise bei Interaktionen
- Verhinderung von Fehlbedienung

---

## 6. Audio & Atmosphäre

> **TL;DR:** Erste Sounds & Musik für Kämpfe, Basis & Umgebung sorgen für Immersion.

- Grundlegende SFX
- Musik zur Stimmung
- Soundeffekte für Kampf & Umgebung

---

## 7. Spielende

> **TL;DR:** Spiel ist gewonnen durch Boss-Kill, verloren durch Zerstörung des Basiskerns.

### Sieg
- Boss wurde besiegt → Spiel gewonnen

### Niederlage
- Basis-Kern zerstört → Spiel verloren
