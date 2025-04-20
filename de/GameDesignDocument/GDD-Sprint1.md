---
title: Game Design Dokument - Sprint 1
description: 
published: true
date: 2025-04-20T12:06:35.065Z
tags: 
editor: markdown
dateCreated: 2025-04-20T12:06:35.065Z
---

# 🎙️ Elevator Pitch – DILIBAD

**DILIBAD** ist ein kooperatives Action-RPG für bis zu 4 Spieler, das Erkundung, Basisbau und Tower Defense zu einem dynamischen Multiplayer-Erlebnis vereint.

Die Spieler entdecken eine fremde Welt, sammeln Ressourcen, befreien Zonen von Gegnern und errichten gemeinsam eine Basis, die sie automatisiert gegen anstürmende Feindwellen verteidigen.  
Mit jedem gesammelten Kristall steigt die Bedrohung – bis schließlich ein mächtiger Boss zum Angriff übergeht. Nur durch Zusammenarbeit und taktisches Vorgehen können die Spieler überleben, das Portal öffnen und den finalen Kampf bestehen.

**Discover. Liberate. Build. Automate. Defend.**  
**DILIBAD – Erobern beginnt mit Zusammenarbeit.**

---

## 🏷️ Tagline

> **„Koop, Kristalle, Katastrophen – vereint überlebt ihr.“**


# 📌 TL;DR – Überblick über dieses Dokument

| Abschnitt                 | Inhalt                                                                 |
|--------------------------|------------------------------------------------------------------------|
| GDD Übersicht            | Ziele und Struktur des Dokuments                                       |
| DILIBAD Bedeutung        | Aufschlüsselung des Namens und Gameplay-Säulen                         |
| Spielkonzept             | Fokus auf Koop, Spieleranzahl, Zielplattformen                         |
| Technologien & Tools     | Übersicht der eingesetzten Systeme & Kommunikationskanäle             |
| Genre                    | Genre-Mix mit Koop-Fokus und Basisverteidigung                         |
| Gameloop                 | Schrittweise Beschreibung des Spielerlebnisses                        |
| Minimale Features        | High-Level Übersicht der für den Prototyp nötigen Systeme              |
| Backup                   | Ursprünglich verfasste Inhalte zur Referenz                            |

---

## 🧾 Zusammenfassung 
**DILIBAD** ist ein kooperatives Action-RPG für bis zu 4 Spieler mit Fokus auf Erkundung, Basisbau und Verteidigung. Das Spielprinzip ist auf einen klar strukturierten Gameloop ausgerichtet, bei dem Spieler Kristalle sammeln, mit ihnen Upgrades freischalten und dabei zunehmend die Aufmerksamkeit feindlicher Kreaturen auf sich ziehen. Die Monster greifen sowohl die Spieler als auch deren Basis an, deren Herzstück – der Kern – unbedingt verteidigt werden muss.

Je mehr Aktionen die Spieler durchführen (z. B. Ressourcenabbau oder Kämpfe), desto stärker reagiert das Spielsystem mit eskalierenden Gegnerwellen. Sobald ein bestimmter Bedrohungslevel erreicht ist, startet der Boss der Karte eine große Angriffswelle. Im späteren Verlauf finden die Spieler drei Schlüsselkristalle, mit denen sie ein Portal aktivieren und den finalen Bosskampf auslösen können. Der Tod ist nicht das Ende – je nach Spielsituation kann es entweder Game Over geben oder eine besonders schwere zweite Chance.

Technologisch basiert das Projekt auf Unity 6 und ist für Windows (und optional WebGL) ausgelegt. Das Projekt nutzt GitHub zur Versionierung, eine eigene Wiki-Seite für die Dokumentation sowie Discord und WhatsApp für die Team-Kommunikation. Die Grafik ist in 2D/2.5D mit Top-Down-Perspektive und 8-Richtungs-Bewegung geplant.

In der ersten spielbaren Version (Sprint 1) liegt der Fokus auf einer minimalen, aber vollständigen Umsetzung der Grundmechaniken. Dazu zählen: Ressourcensammeln, Upgrades, Gegnerwellen, Basisverteidigung, Portalaktivierung, Bosskampf sowie einfache Sieg-/Niederlagebedingungen. Alle darüber hinausgehenden Features werden in einem separaten GDD beschrieben. Dieses Dokument bildet somit die Grundlage für Planung, Umsetzung und spätere Erweiterung.

---


# 📄 GDD Übersicht – DILIBAD

In diesem Dokument wird das Spielkonzept von **DILIBAD** festgehalten – mit allen relevanten Informationen, die **für das Ende von Sprint 1** geplant und vorgesehen sind.

Alle weiterführenden Features und Konzepte, die **über Sprint 1 hinausgehen**, werden in einem **separaten GDD** dokumentiert.

## 🎯 Ziel dieser Trennung
- Klare Abgrenzung des **aktuellen Umfangs**
- Bessere Einschätzung des **Arbeitsaufwands**
- Verständlichere Vorstellung der **spielbaren Version** zum Ende von Sprint 1

---

## 🔠 Was bedeutet DILIBAD?

**DILIBAD** steht für die fünf zentralen Gameplay-Säulen:

- **DI** = Discover  
- **LI** = Liberate  
- **B**  = Build  
- **A**  = Automate  
- **D**  = Defend  

---

## 👥 Spielkonzept

- **Multiplayer-First Design**  
- Erste Version: **bis zu 4 Spieler kooperativ spielbar**

---

## 🖼️ Grafik & Perspektive

- **2D / 2.5D** Top-Down View  
- Mit **8-Richtungs-Bewegung**

---

## 🖥️ Plattformen

- **Windows** (Primärplattform)  
- **WebGL** *(optional, abhängig von Performance & Stabilität)*

---

## 🛠️ Technologien & Tools

- **Unity 6** *(genaue Version hier eintragen)*  
- **GitHub** für Versionskontrolle  
- **[wiki.dilibad.de](https://wiki.dilibad.de)** für Projektdokumentation  
- **Discord** für Projektkommunikation  
- **WhatsApp** für schnelle und wichtige Kommunikation

---

## 🎮 Genre

- **Coop Action-RPG**  
- Mit Elementen von **Base-Building** und **Tower Defense**



### 🌀 **Gameloop – Übersicht**

#### 1. **Ressourcen sammeln & Aufrüstung**

- Die Spieler sammeln **Kristalle**, die sie für **Charakter-** und **Basis-Upgrades** einsetzen können.
    
- Das **Sammeln von Kristallen** zieht nach und nach die **Aufmerksamkeit von Monstern** auf sich.
    
- Während des Abbaus kann es zu **vereinzelten Monsterangriffen** kommen.
    

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


# 🎮 Minimal Gameloop – High-Level Feature- und Systemübersicht

Diese Übersicht beschreibt alle notwendigen Systeme und Bestandteile, um die Kern-Gameloop in minimaler Form umzusetzen – losgelöst von technischen Details.

---

## 🔷 Spieler-Elemente
- Spielercharakter mit Bewegungs- und Interaktionsmöglichkeiten  
- Ressourcensystem: Kristalle sammeln und verwalten  
- Upgradesystem: Einfache Verbesserungen (z. B. Schaden, Bewegung)  
- Inventarsystem: Verfolgung von normalen und besonderen Kristallen  
- Respawn-Mechanik: Spieler kehrt nach dem Tod zur Basis zurück  

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
- (Optional) Basisausbau: Minimaler Ausbau durch Ressourcen möglich (Feste slots nur werte und visuelle darstellung verändern)  

---

## 🔮 Portal- und Fortschrittssystem
- Portalstruktur: Spezieller Ort, der geöffnet werden kann  
- Schlüsselkristalle: Drei besondere Ressourcen zum Aktivieren des Portals  
- Portalaktivierung: Ermöglicht Zugang zum finalen Bosskampf  

---

## 🧠 Spielregeln & Zustände
- **Siegbedingung:** Boss oder finale Welle besiegt  
- **Niederlagebedingungen:**  
  - Basis-Kern zerstört  
  - Alle Spieler beim Bosskampf dauerhaft besiegt  
- Zustandswechsel:  
  - Ressourcen sammeln  
  - Verteidigen  
  - Portal öffnen  
  - Bosskampf  

---

## 🌍 Spielwelt & Umgebung
- Kristallquellen: Interaktive Abbaupunkte  
- Spawnpunkte für Gegner  
- Positionierte Bossarena / Bossspawnpunkt  
- Erkundbare Welt mit Schlüsselorten (Basis, Portal, Kristallfelder)  

---

## 🧭 Benutzeroberfläche & Feedback
- Ressourcenanzeige (Kristalle)  
- Upgrade-Button & Statusanzeige  
- Aufmerksamkeit des Bosses (z. B. Balken oder % Anzeige)  
- Übersicht über gesammelte Schlüsselkristalle  
- Sieg- / Game Over-Bildschirm  

---







Backup Unverarbeitete Versionen:

In diesem GDD Dokument wird das Spielkonzept von DILIBAD festgehalten mit allen relevanten Informationen die für die Version an Ende des Sprints 1 vorhanden sind. Alle darüber hinausgehenden Features und Konzepte werden in einem Seperaten GDD festgehalten.

Diese trennung soll es ermöglichen den aktuellen aufwand des Konzeptes besser greifen zu können und sich die erste Spielbare Version am Ende von Sprint 1 besser vorstellen zu können.


DILIBAD  ist die Abkürzung für Discover. Liberate. Build. Automate. Defend

setzt sich zusammen aus:
DI = Discover
LI =  Liberate
B = Build
A = Automate
D = Defend


Das Spiel ist als Multiplayer first Konzept geplant und soll in der ersten Version 4 Spieler unterstützen.

Grafiken 2D / 2.5D in Top Down mit 8 Directional Movement

Plattformen: Windows + evtl. WebGL

Technologien:
Unity 6 (Version genau hier  eingeben)
Github Version Controll
wiki.dilibad.de => Doku
Discord => Projekt Kommunikation
Whatsapp => Schnelle wichtige Kommunikation


**Genre:**  
Coop  | Action RPG |  "Base-Building" (Tower Defense)

Gameloop:

Die Spieler sammeln Kristalle, die er für Upgrades am Charakter und der Basis nutzen kann.
Durch das sammeln von Kristallen erregen die Spieler sie Aufmerksamkeit von Monstern.
Vereinzelnd greifen Monster beim Rohstoffe sammeln Spieler an.
Durch das Abbauen der Kristalle und das bekämpfen erregen die Spieler die Aufmerksamkeit des Bosses der Karte auf sich. 
Wenn der Boss 100% Aufmerksamkeit hat, schickt er eine Angriffswelle auf die Basis. Die Basis hat einen Kern der die Monster anzieht. 

Die Spieler müssen die Basis verteidigen. 
Durch das Sammeln von Kristallen können Spieler ihre eigene Ausrüstung verbessern oder die Basis verbessern, sodass sich diese Autonomer verteidigen kann. 

Die Spieler finden ein Portal das durch 3 besondere Kristalle geöffnet werden kann.

Wenn die Spieler diese 3 Kristalle gefunden haben, können Sie das Portal aktivieren und den Boss bekämpfen.

Wenn die Spieler sterben werden sie in der Basis neu gespawned. 

Wenn die Spieler als Gruppe beim Boss sterben und ihn nicht besiegen konnten:
Option A ) Game Over
Option B ) Spawnen bei der Basis und eine besonders starke Welle wird vom Boss geschickt. Beispielsweise durch Miniboss oder deutlich mehr Gegner.

Option B ist kein Dead End und man kann sich eine "zweite" Chance erspielen.

Wenn der Boss besiegt wurde ist das Spiel gewonnen und die Runde zuende.
Wenn der Kern der Basis zerstört wird ist das Spiel mit Game Over beendet.