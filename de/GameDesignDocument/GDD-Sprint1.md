---
title: Game Design Dokument - Sprint 1
description: 
published: true
date: 2025-04-26T16:15:31.907Z
tags: 
editor: markdown
dateCreated: 2025-04-20T12:06:35.065Z
---


# Disclaimer 
> In diesem Dokument liegt der Fokus auf den Gameplay-Mechaniken, die für den ersten Sprint benötigt werden. Erweiterungsoptionen werden separat dokumentiert und bei Bedarf in spätere Versionen des GDD (Game Design Document) integriert. Ziel ist es, eine übersichtliche und verständliche Dokumentation sicherzustellen, sodass alle Entwickler ausschließlich die für den aktuellen Sprint relevanten Informationen aufnehmen und ein klares Verständnis der benötigten Funktionen gewinnen können.

--- 
# 📋 Steckbrief 

## Genre
- Kooperatives Action-RPG  
- Mit Base-Building und Tower Defense-Elementen

## Plattformen
- Windows (Primär)
- WebGL (optional)

## Spieleranzahl
- 4 Spieler (kooperativ)

## Perspektive & Grafik
- 2D / 2.5D Top-Down
- 8-Richtungs-Bewegung

## Technologie & Tools
- Unity 6 (genaue Version folgt)
- GitHub (Versionierung)
- Eigene Wiki-Seite für Dokumentation > Synced automatisch auf Github
- Discord & WhatsApp für Kommunikation

## Gameplay-Säulen
- **Discover** – Erkunden
~~- **Liberate** – Befreien~~
- **Build** – Basis bauen & aufwerten
~~- **Automate** – Automatisieren~~
- **Defend** – Verteidigen

## Zusammenfassung
Kooperativ Ressourcen sammeln, eine Basis errichten und automatisiert gegen eskalierende Gegnerwellen verteidigen. Ziel ist es, ein Portal zu aktivieren und den finalen Boss zu bezwingen.


---

# 🎙️ Elevator Pitch

**DILIBAD** ist ein kooperatives Action-RPG für bis zu 4 Spieler, das Erkundung, Basisbau und Tower Defense zu einem dynamischen Multiplayer-Erlebnis vereint.

Die Spieler entdecken eine fremde Welt, sammeln Ressourcen, befreien Zonen von Gegnern und errichten gemeinsam eine Basis, die sie automatisiert gegen anstürmende Feindwellen verteidigen.  
Mit jedem gesammelten Kristall steigt die Bedrohung – bis schließlich ein mächtiger Boss zum Angriff übergeht. Nur durch Zusammenarbeit und taktisches Vorgehen können die Spieler überleben, das Portal öffnen und den finalen Kampf bestehen.

**Discover. Liberate. Build. Automate. Defend.**  
**DILIBAD – Erobern beginnt mit Zusammenarbeit.**

---





### 🌀 **Gameloop – Übersicht**

#### 1. **Ressourcen sammeln & Aufrüstung**

- Die Spieler sammeln **Kristalle**, die sie für **Charakter-** und **Basis-Upgrades** einsetzen können.
    
- Das **Sammeln von Kristallen** zieht nach und nach die **Aufmerksamkeit von Monstern** auf sich.
    
- Während des Abbaus kann es  auf der Karte zu **vereinzelten Monsterangriffen** kommen.
    

#### 2. **Gefahr eskaliert**

- Durch das **fortwährende Sammeln** und **Kämpfen** erhöht sich die **Aufmerksamkeit des Kartenbosses**.
    
- Sobald der Boss **100 % Aufmerksamkeit** erreicht, startet er eine **Angriffswelle** auf die **Spielerbasis**.
    
- Die Basis enthält einen **Kern**, der die Monster anzieht – er ist das Hauptziel der Angreifer.
    

#### 3. **Basisverteidigung**

- Die Spieler müssen die **Basis und ihren Kern** gegen die Wellen verteidigen.
    
- Kristalle können für:
    
    - **Ausrüstungs-Upgrades** (Spieler)
        
    - **Defensiv-Upgrades** (Basis)  
        verwendet werden.
        
- Die Basis kann dadurch **teilweise autonom** gegen Feinde vorgehen.
    

#### 4. **Das Portal & der Bosskampf**

- Auf der Karte gibt es ein **Portal**, das nur durch **3 besondere Kristalle** geöffnet werden kann.
    
- Sobald die Spieler diese **drei Schlüsselkristalle** gefunden haben, können sie:
    
    - das **Portal aktivieren**
        
    - und sich dem **Bosskampf** stellen.
        

#### 5. **Sterben & Respawn**

- Wenn Spieler sterben, werden sie **in der Basis respawned**.
    
- Wenn **alle Spieler im Bosskampf** sterben, gibt es zwei Optionen:
    
    - **Option A:** _Game Over_
        
    - **Option B:** Die Spieler respawnen bei der Basis, jedoch folgt eine **besonders starke Angriffswelle** (z. B. mit Miniboss oder vielen Gegnern).
        
        - Diese Option bietet eine **"zweite Chance"** – das Spiel ist noch nicht verloren.
            

#### 6. **Spielende**

- Das Spiel endet mit einem **Sieg**, wenn der **Boss besiegt** wurde.
    
- Das Spiel endet mit einem **Game Over**, wenn der **Kern der Basis zerstört** wurde.


# 🎮 Feature- und Systemübersicht

Diese Übersicht beschreibt alle notwendigen Systeme und Bestandteile, um die Kern-Gameloop im ersten Sprint umzusetzen. 

---

## 🔷 Spieler
- Spielercharakter mit Bewegungs- und Interaktionsmöglichkeiten  
- Ressourcensystem: Kristalle sammeln und verwalten  
- Upgradesystem: Verbesserung des Angriffs, der Verteidigung oder der Klasse
- Inventarsystem: Verfolgung von normalen und besonderen Kristallen  
- Respawn-Mechanik: Spieler kehrt nach dem Tod zur Basis zurück  

## 🧑‍🤝‍🧑 Spielerklassen (Sprint 1)
- Zwei Klassen mit unterschiedlichen Rollen zur Förderung von Teamplay
- Klassen beeinflussen unterschiedliche Aspekte der Gamemechaniken

## 🧺 Sammler
Rolle: Fokus auf effizientes Sammeln von Rohstoffen

Upgradeoption:
- Schnelleres Abbauen von Rohstoffen


## 🗡️ Kämpfer
Rolle: Fokus auf den Kampf gegen Monster

Upgradeoption:
- Höhere Angriffsgeschwindigkeit


---

## 🧪 Gegner- & Gefahrensysteme
- Grundgegner: Greifen Spieler und Basis an  
- Wellenmechanik: Gegner erscheinen in Gruppen und greifen an  
- Aufmerksamkeitssystem: Spieleraktionen erhöhen eine Bedrohungsanzeige  
- Bosswelle: Bei 100 % Aufmerksamkeit startet eine starke Angriffswelle  

---

## 🏰 Basis- und Verteidigungselemente
- Spielerbasis: Rückzugs- und Verteidigungspunkt  
- Basiskern: Zentrales Ziel der Gegner – Zerstörung führt zu Game Over  
- Verteidigung durch Spieler: Aktive Abwehr, primitive Tower mit Single target 
- Bereits platzierte Stukturen können aufgewertet werden
- Leere Slots können durch den Spieler befüllt werden und stellt den Ausbau der Basis da

---

## 🔮 Portal- und Fortschrittssystem
- Portalstruktur: Spezieller Ort, der geöffnet werden kann  
- Schlüsselkristalle: Drei besondere Ressourcen zum Aktivieren des Portals  
- Portalaktivierung: Ermöglicht Zugang zum finalen Bosskampf auf einer seperaten Karte

---

## 🧠 Spielregeln & Zustände
- **Siegbedingung:** Boss besiegt  
- **Niederlagebedingungen:**  
  - Basis-Kern zerstört  
  - Alle Spieler beim Bosskampf dauerhaft besiegt (Optional) 
- Zustände:  
  - Erkundung und Rohstoffgewinnung 
  - Aktive Angriffswelle  
  - Portal öffnen  
  - Bosskampf  

---

## 🌍 Spielwelt & Umgebung
- Kristallquellen: Interaktive Abbaupunkte  
- Spawnpunkte für Gegner  
- Seperate Karte für den Boss
- Erkundbare Welt mit Schlüsselorten (Basis, Portal, Kristallfelder)  

---

## 🧭 Benutzeroberfläche & Feedback
- Ressourcenanzeige (Kristalle)  
- Upgrade-Button & Statusanzeige  
- Aufmerksamkeit des Bosses (z. B. Balken oder % Anzeige)  
- Übersicht über gesammelte Schlüsselkristalle  
- Sieg- / Game Over-Bildschirm  
