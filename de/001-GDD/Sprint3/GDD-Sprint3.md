---
title: Game Design Document - Sprint 3
description: 
published: true
date: 2025-07-05T14:30:38.057Z
tags: 
editor: markdown
dateCreated: 2025-07-04T15:01:47.369Z
---

# DILIBAD

# Gegnerfraktionen

Aktuell sind drei unterschiedliche Gegnerfraktionen implementiert:

### Northmarchborn  
Barbarenartige Gegner mit Fokus auf Nahkampf und Fernkampfangriffe:
- Standardgegner: Barbar, Bogenschütze, Nomade, Hexendoktor, Elite-Bogenschütze, Berserker  
- Miniboss: Oger  
- Hauptboss: Großer, golemartiger Barbar

### Graveborn  
Skelett-basierte Gegner mit vielfältigen Klassen:
- Standardgegner: Skelettkrieger  
- Weitere Gegner: Bogenschütze, Magier, Berserker  
- Miniboss: Skelett-Ritter  
- Hauptboss: Großes, brutales Skelett

### Soulforge  
Dunkle Skelettfraktion mit besonderen Fähigkeiten:
- Standardgegner: Dunkler Bogenschütze, Nekromant  
- Miniboss: Todeslord (großes Skelett mit Rüstung und flammendem Schwert)  
- Hauptboss: Todesritter mit Unsichtbarkeits- und Überraschungsangriffsfähigkeiten

## Besondeheit: Gigantischer Boss
Ein einzigartiger Endboss mit speziellen Animationen und Fähigkeiten. Statt sich frei zu bewegen, agiert er als stationäres CombatBuilding. Seine Angriffe umfassen großflächige Schläge auf die Arena, z.B. durch Zuschlagen mit den Armen oder Zusammenschlagen der Hände, um massive Schadenszonen zu erzeugen.

---

# NPC State Machine

Jeder NPC im Spiel wird durch eine State Machine gesteuert, die sieben verschiedene Zustände (States) umfasst:

- **Idle State:**  
  Die NPCs haben kein aktuelles Ziel, bewegen sich nicht und befinden sich in einem Ruhezustand. Während dieses States steigt ein "Langeweile"-Wert, der nach Erreichen eines bestimmten Schwellenwerts den Wechsel in den Wandering State auslöst.

- **Wandering State:**  
  NPCs bewegen sich zu einem neuen Zielpunkt, um ihre Umgebung zu erkunden und lebendiger zu wirken. Dieser State legt lediglich das Bewegungsziel fest, das eigentliche Bewegen wird im Movement State ausgeführt.

- **Movement State:**  
  Verantwortlich für die generelle Fortbewegung der NPCs zum festgelegten Zielpunkt.

- **PickSkill State:**  
  Die NPCs wählen in regelmäßigen Abständen eine Fähigkeit aus und bereiten diese vor.

- **Attack State:**  
  In diesem State führen die NPCs die ausgewählten Fähigkeiten aus und bewegen sich dabei aktiv auf Spieler zu.

- **Enrage State (boss-spezifisch):**  
  Dieser State stellt einen Damage-Check dar. Nach Entdeckung des ersten Spielers startet ein Timer. Läuft dieser ab, ohne dass der Boss besiegt wurde, wird ein mächtiger Angriff ausgelöst, der die gesamte Arena betrifft. Die Spieler haben eine letzte Chance, während der Ladephase des Angriffs, den Boss mit maximalem Schaden zu besiegen. Schafft der Boss es, den Angriff durchzuführen, wird die gesamte Spielergruppe besiegt. Dieser Mechanismus sorgt für erhöhte Spannung und fordert gutes Teamplay.

- **Invulnerability (Invuln) State (boss-spezifisch):**  
  In bestimmten Lebensabschnitten wird der Boss unbesiegbar und führt eine Weltaktion durch — eine Mechanik, die von den Spielern Ausweichmanöver erfordert. Nach Abschluss dieser Phase ist der Boss wieder verwundbar.

---


# Skilltypen und deren Einsatz

## Projektil-Skills  
Ermöglichen das Abschießen von Geschossen in verschiedene Richtungen. Hauptsächlich von Bogenschützen und Magiern genutzt. Spieler können gezielt auf Gegner schießen, während Gegner automatisierte Zielverfolgung verwenden.

## Turret- und Deployable-Skills  
Skills zum Platzieren stationärer Objekte wie Türme, die eine bestimmte Zone kontrollieren und Schaden verursachen. Hauptsächlich vom Engineer-Spielercharakter verwendet.

## Flächen-Effekte (AoE-Skills)  
Fähigkeiten, die an einer Zielposition aktiviert werden und Schaden in einem festgelegten Radius verursachen. Beispielsweise von Rittern zur Schadensverteilung an mehreren Gegnern genutzt.

## Nahkampf-Skills  
Nahbereichsangriffe mit Animationen und zeitlichen Verzögerungen. Diese Fähigkeiten sind Standard für Nahkampfeinheiten und fördern taktische Nahkampfsituationen. Variationen mit stärkeren Angriffen gibt es bei Engineer und Ritter.

## Heil-Skills  
Fähigkeiten zur Regeneration von Lebenspunkten bei Verbündeten im Wirkungsbereich. Unterstützen Teamplay und Überlebensfähigkeit, meist vom Magier/Heiler eingesetzt.

## Buff- und Debuff-Skills  
Temporäre Verstärkung von Verbündeten (Buffs) oder Einschränkung von Gegnern (Debuffs). Magier/Heiler nutzen diese Fähigkeiten, um das Team zu stärken und Gegner zu schwächen. Der Engineer verwendet Buffs, um seine Türme zu verbessern.

## Spezielle Skills  
Einzigartige Fähigkeiten, z.B.:  
- Unsichtbarer AoE-Angriff: Teleportation zum nächsten Gegner gefolgt von einem AoE-Schaden (Boss-spezifisch).  
- Dash-Angriff: Schnelles Zuschlagen beim weit entferntesten Gegner (z.B. Golem).  
- Kreisförmiger Projektilangriff: Mehrere Pfeile werden in einer kreisförmigen Formation abgefeuert, genutzt von Bogenschützen und in höheren Schwierigkeitsgraden auch von Gegnern.

---

Diese vielfältigen Skilltypen bieten ein breites taktisches Spektrum und sorgen für abwechslungsreiche, dynamische Kampfmechaniken. Jeder Boss besitzt einzigartige Fähigkeiten, die seine Herausforderung und Persönlichkeit unterstreichen.

---


## 5. Map Design

### 5.1 Verwendete Assets

Für die Kartenlayouts wurde das Asset-Paket **„The Depths v1.5.1“** von Rafael Matos verwendet. Dieses bietet modulare Plattform- und Höhlenstrukturen, die sich für verwinkelte, mehrstufige Level mit vertikalen und horizontalen Elementen eignen.

### 5.2 Kartenübersicht

Das Spiel verwendet zwei Hauptkarten mit jeweils einer Variante, um Abwechslung und unterschiedliche Spielerfahrungen zu ermöglichen:

- **Great Depths** (horizontaler Fokus)  
- **Smaller Depths** (vertikaler Fokus)

### 5.3 Great Depths

#### Aufbau und Zielsetzung

Diese Karte legt den Fokus auf horizontale Erkundung. Der Spieler wird durch bewusst gesetzte Pfade und Plattformverbindungen durch die Umgebung geführt. Dabei sollen Neugier und Entdeckerlust durch das Leveldesign belohnt werden.

#### Designmerkmale

- **Erkundungspfadstruktur:** Spieler können zwischen mehreren Pfaden wählen, was ein gewisses Maß an Entscheidungsfreiheit erlaubt.  
- **Ressourcenverteilung:** Wer Erkundung betreibt, wird mit Ressourcen (z. B. Heilung, Upgrades oder Währung) belohnt.  
- **Backtracking-Möglichkeiten:** Schlaufen und alternative Routen erlauben Rückkehr zu früheren Abschnitten.  
- **Plattformgestaltung:** Es gibt sowohl größere Plattformen für Kampf- und Ruhebereiche als auch kleinere Übergangsplattformen.

#### Bossstruktur

- **Hauptboss** auf der obersten Plattform (Endpunkt der Karte).  
- **Mini-Boss** auf einer kleineren Plattform darunter als optionaler Zwischengegner.

#### Kartenvariation

- **Veränderte Wege:** In dieser Variation sind einige Verbindungswege und Plattformzugänge unterbrochen oder blockiert.  
- **Ziel:** Den Spieler zwingen, neue Routen zu wählen und so die Wiederholung von Durchläufen abwechslungsreicher zu gestalten.

### 5.4 Smaller Depths

#### Aufbau und Zielsetzung

Diese Karte besitzt einen stärkeren vertikalen Aufbau und ist kompakter als die *Great Depths*. Die Erkundungsmöglichkeiten sind eingeschränkter, das Level fokussiert sich stärker auf Kampfintensität und vertikale Progression.

#### Designmerkmale

- **Vertikale Struktur:** Der Spielfortschritt erfolgt überwiegend von oben nach unten.  
- **Begrenzte Erkundung:** Es gibt nur wenige alternative Pfade.  

##### Gegnerdichte

- Im **unteren Bereich** konzentrieren sich vermehrt Gegnerhorden, was die Schwierigkeit steigert.  
- Im **oberen Bereich** befinden sich drei kleinere Plattformen, auf denen entweder ein Boss oder ein Mini-Boss platziert werden kann.

#### Kartenvariation

- **Startpunkt in der Kartenmitte:** Der Spieler beginnt in der Mitte der Karte, wobei der Zugang zum unteren Bereich blockiert ist.  
- **Ziel:** Diese verkürzte Variante ermöglicht einen schnellen Bosskampf mit minimaler Erkundung.  
- **Einsatz:** Ideal für Speedruns, Herausforderungsläufe oder Storyabschnitte mit Zeitdruck.

