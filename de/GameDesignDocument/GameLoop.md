---
title: GameLoop
description: 
published: true
date: 2025-04-18T10:10:24.025Z
tags: 
editor: markdown
dateCreated: 2025-04-17T21:49:27.280Z
---

# 🔁 Gameloop – Spielablauf in DILIBAD

## TL;DR

- 🔨 Rohstoffe abbauen → Basis errichten & Ausrüstung herstellen  
- ⚠️ Ressourcenabbau → triggert Gegnerwellen  
- 🛡️ Gegner besiegen → Loot einsammeln & reparieren  
- 🌍 Gebiet erweitern → Hinweise zum Boss entdecken  
- 🧠 Ausrüstung optimieren → Boss besiegen & Level abschließen  

---

## 🧩 Ausführlicher Ablauf

### 1. ⛏️ Ressourcen sammeln
Der Spieler beginnt damit, **Rohstoffe in der Welt abzubauen** – z. B. Holz, Stein oder Metalle.  
Diese Materialien dienen als Grundlage für den Aufbau der eigenen [Basis](./features/Bauen/Bausystem.md), neue [Ausrüstungen](./features/Entitaeten/Spieler.md) oder [Verteidigungsanlagen](./features/Bauen/Bausystem.md).

> **Hinweis:** Je mehr Rohstoffe gesammelt werden, desto stärker und schneller werden Gegnerwellen aktiviert.

---

### 2. ⚠️ Gegnerwellen & Verteidigung
Jede Ressourcengewinnung zieht **eine neue Gegnerwelle** an.  
Sobald die Welle ausgelöst wird, muss der Spieler seine Erkundung abbrechen und sich auf die Verteidigung konzentrieren.

- Aktives Kampfsystem mit Bewegung, Blocken, Skills
- Unterstützung durch [Tower](./features/Bauen/Bausystem.md) und automatisierte Abwehrsysteme
- Ressourcenmanagement und [Aktionen](./features/Aktionen/Aktionen.md) entscheidend für das Überleben

---

### 3. 🧹 Nach der Welle
Nach dem erfolgreichen Abwehren einer Welle:
- Kann der Spieler **Loot aufsammeln** (Ressourcen, Materialien, Ausrüstungsteile)
- Wird die **Basis repariert oder erweitert**
- Beginnt ein neuer Kreislauf mit erneutem Abbau

Mit jedem Zyklus vergrößert sich der **zugängliche Bereich der Karte**, neue Rohstoffvorkommen und Hinweise werden freigeschaltet.

---

### 4. 🔍 Hinweise & Bosskampf-Vorbereitung
Durch Erkundung kann der Spieler **Hinweise** auf den Standort des Boss-Monsters finden.  
Gleichzeitig bereitet er sich vor:
- Verbessert seine Ausrüstung (Waffen, Rüstung, [Fähigkeiten](./features/Entitaeten/Spieler.md))
- Stärkt die Basis, um letzte Wellen zu überstehen
- Optional: Automatisierung für spätere Phasen aufbauen

---

### 5. 💥 Bosskampf & Abschluss
Sobald alle Hinweise gesammelt und die Vorbereitungen abgeschlossen sind, kann der Spieler den Bossbereich betreten.  
Dort erwartet ihn ein **einmaliger, stufenbasierter Bosskampf** mit einzigartigen Mechaniken und Mustern.

> **Wird der Boss besiegt, gilt das Gebiet als befreit und das Level ist abgeschlossen.**

Der Fortschritt wird gespeichert, Belohnungen werden verteilt – und das nächste Gebiet kann betreten werden.

---

*Der Gameloop von DILIBAD basiert auf einem dynamischen Wechselspiel zwischen Risiko, Ressourcen, Kampf und strategischem Ausbau.*
