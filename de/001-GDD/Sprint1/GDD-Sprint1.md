---
title: Game Design Dokument - Sprint 1
description: 
published: true
date: 2025-04-28T12:27:27.560Z
tags: verifizierung_ausstehend
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
Zu beginn des Spielers kann dieser aus einer Auswahl von Klassen auswählen welche seine Spezialisierung in der Runde bestimmt. Als nächstes wählt der Spieler seine Waffe aus, welche ihm Angriffsfähigkeiten für den Kampf zur Verfügung stellt. Jede Klasse kann im Kampf mitwirken.
### Charakter General
- Bewegung: **WASD**
- Ausweichen: **Leertaste**
- Kampf option A: **Skill 1: Left Click,Skill 2: Right Click ,Skill 3: Q, Skill 4: E**
- Kampf option A: **Skill 1: Alpha1,Skill 2: Alpha2 ,Skill 3: Alpha3, Skill 4: Alpha4**
### Charakter Stats
- Health
- Damage
- Defense
- Attack Speed (nur für Auto Attack)
- Movement Speed
- Abbaugeschwindigkeit
- Klasse (Spezialisierung, festgelegt)

### Klassen
| Klasse    | Rolle | Upgradeoption |
|:----------|:------|:--------------|
| **Sammler** | Fokus auf Ressourcensammeln | Schnelleres Abbauen |
| **Kämpfer** | Fokus auf Kampf | Höhere Angriffsgeschwindigkeit |

### Inventar
- Maximale Tragemenge an Kristallen

### Items
- Kristall
- Hinweise

### Kampfarten
- **Nahkampf (Schwert)**
  - Auto Attack (abhängig von Attack Speed)
  - Schwerer Angriff (Skill Cooldown)
  - Rundumschlag (Skill Cooldown)
  - Jump Attack (Skill Cooldown)

- **Fernkampf (Bogen)**
  - Auto Attack (abhängig von Attack Speed)
  - Geladener Schuss (Skill Cooldown)
  - AoE-Fähigkeit: Pfeilhagel (Skill Cooldown)
  - Dodge Backward + Pfeilschuss (Skill Cooldown)

### Charakter Upgrades
- 3 passive Upgrades:
  - Attack
  - Defense
  - Sammler: Schnelleres Abbauen | Kämpfer: Attack Speed

### Interaktion
- Rohstoffe abbauen und Gebäude interagieren über **Interaktion Key** (bei Nähe)

---

## 👾 Gegner- und Gefahrensysteme

### Boss & Monster
- **Boss Angriffsmuster**:
  - Base Attack
  - Heavy Attack (Cooldown)
  - AoE Attack (Cooldown)
  - Knockback (Cooldown)

- **Angriffswellen**
  - Jede neue Welle wird stärker:
    - Stärkere Monster
    - Mehr Monster

### Gefahrenlevel
- Zeigt an, wann die nächste Angriffswelle kommt.
- Steigt durch:
  - Abbauen von Rohstoffen
  - Töten von Monstern

- (TBD) Zusätzlicher fixer Anstieg über Zeit (für Schwierigkeitsgrad-Varianten).
- Exakte Werte werden durch Tests bestimmt.

### Aggro Level
- Einfaches **Enmity System** 
- Ziel: Spieler können Monster nicht abusiv kiten.


[Enmity System am Beispiel von Final Fantasy](https://ffxiv.consolegameswiki.com/wiki/Enmity)

---

## 🏰 Basis- und Verteidigungssysteme
- **Rohstoffmanagement**
  - Rohstoffe zur Basis bringen.
  - Kristalle für Charakter-Upgrades und Bau/Aufwertung von Strukturen verwenden.

- **Wände und Türme**
  - **Walls:** Upgrade erhöht Lebenspunkte.
  - **Türme:** Upgrade erhöht Lebenspunkte, Angriffsschaden und Attack Speed.

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
![hauptmenu06.png](/hauptmenu06.png)

- Ressourcenanzeige (Kristalle)
- Upgradebuttons mit Status
- Bedrohungsanzeige (Balken oder Prozent)
- Anzeige gesammelter Schüsselkristalle
- (Sieges- und Game Over-Screen)

Bei der Findung und Validierung der anfänglichen UI-Elemente im Projekt DILIBAD wurde sich an den gängigen Spielen der Genres Aufbau und Rollenspiel (RPG) orientiert.
Für die Ansicht, die der Spieler nach dem Öffnen des Spiels zu Gesicht bekommt, hat man sich für eine Begrüßung des Spielers inklusive dem Spielnamen „DILIBAD“ und einem Hauptmenü mit folgenden Unterpunkten entschieden: „Neues Spiel“, „Spiel laden“, „Einstellungen“ und „Spiel verlassen“.

Über das Menü, das der Spieler während des Spielens aufrufen kann, sollten die Funktionen „Speichern“, „Upgrades“, „Graphics“-, „Lautstärkeeinstellungen“ und „Spiel verlassen“ aufrufbar sein.
![menu.png](/menu.png)

Als essenziell für die grundlegende Spielansicht für den Benutzer haben sich die UI-Elemente „Lebenspunkte“, „Schlüsselkristalle im Inventar“, „globale Anzahl der Schlüsselkristalle im Teambesitzt“, „Anzahl der Hauptkristalle im Inventar mit Tragekapazität“ und „Miniaturansicht der Karte“ mit anliegender „Enmity-Anzeige“ herausgestellt.

![ingame.png](/ingame.png)
Des Weiteren hat man sich für das Implementieren einer „Fähigkeitenleiste“ mit Tastenkürzeln für die Fähigkeiten des Charakters sowie den Knopf zum Öffnen des Einstellungsmenüs.
Im weiteren Vorgehen werden die Funktionen auf ihre Benutzerfreundlichkeit getestet und abhängig von den Ergebnissen erweitert, verändert oder entfernt.
