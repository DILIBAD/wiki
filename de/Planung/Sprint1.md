---
title: Sprint 1
description: 
published: true
date: 2025-04-18T23:14:28.702Z
tags: 
editor: markdown
dateCreated: 2025-04-18T23:14:28.702Z
---

# Übersicht
Zu Ende von Sprint 1 soll eine simple Version der Gameloop im Multiplayer umgesetzt sein.





## Funktionalitäten

Lore Intro als Video durch Bilder mit Untermalung von Audio
Hauptmenü
Tutorial
GameSession

### UI
Der Spieler kann seine Leben sehen
Der Spieler sieht Cooldown auf Skills
Der Spieler sieht seine Skills
Der Spieler sieht einen Indikator für die Angriffswelle
Der Spieler sieht wenn das Spiel gewonnwn wurde
Der Spieler sieht wenn das Spiel verloren wurde
Der Spieler sieht den verursachten Schaden
Der Spieler kann Leben der Gegner sehen
Der Spieler kann sehen welche Waffe hergestellt werden kann
Der Spieler kann sehen wie viel Materialien in der Base sind
Der Spieler kann sehen wie viel Materialien benötigt werden um ein Upgrade durchzuführen
Der Spieler kann sehen wie viel Materilaen benötigt werden um etwas zu bauen

### UX
Spieler muss sehen wenn Aktionen möglich oder nicht Möglich sind.
Spieler muss sehen wenn Interaktionen möglich sind


### Music & Atmospähre
Es werden Grundlegende SFX und Musikelemente benötigt sodass eine Immersion entstehen kann

### Basis
Verbessern der Basis
Anfertigen von besserem Equipment zu Progression Zwecken
Verteidigungsstrukturen
Basis Kern

### Angriffswellen
Durch ein Indiklator wird angezeigt wie viel Zeit zur nächsten Angriffswelle verbleibt. 
Der Indikatot ist nicht Zeitgebunden sondern Aktiongebunden
Wenn eine Angriffswelle ausgelöst wird, kommt eine bestimmte Anzahl an Wellen mit vorgegebenen Gegnern.
Die Gegner spawnen an verschiedenen Punkten und greifen die Basis des Spieler an. 
Nebenziele die auf dem Weg zur Basis gefunden werden, werden angegriffen.
Angriffswellen werden im Verlauf der Session schwerer.

### Kampfsystem
Für das Kampfsystem werden Indikatoren benötigt die Anzeigen wo ein Skill gecastet wird.
Für das Kampfsystem müssen gegnerische Attaccken visualisiert werden, sodass der Spieler diesen ausweichen kann.

### Spieler
Auswahl zwischen verschiedenen Klassen (Tank, Melle DPS, Healer/Support, Ranged)
Jede Klasse kommt mit mehreren Angriffen
Spieler können Rohstoffe einsammeln
Spieler können Loot aufheben
Spieler können laufen
Spieler können Aktionen durchführen, welche die Angriffswelle näher rücken lässt.
Spieler spawnen in der Basis neu wenn sie gestorben sind (Cooldown/Respawnzeit)
Spieler können ohne Ziel angreifen
Spieler können Casten
Spieler können Zielpunkt für Skill festlegen


### NPCs
Es wird zwischen freundlichen und feindlichen NPCs unterschieden.
Beide NPCs nutzen das gleiche StateMachine System und teilen sich alle States
Basierend auf Konfiguration von Sensoren werden unterschiedliche Entitäten als Fein erkannt.
NPCs haben grundlegende Angriffsmuster
Die Einstellungen der Stats einer Einheit können Konfiguriert werden.

#### Friendly NPCs
Es gibt eine erste Itteration von freundlichen NPCs welche Teil der Basis sind


#### Enemy NPCs
Es gibt eine gegnerische Monster Eintät welche innerhalb der Angriffswellen genutzt wird.

### Boss NPCs
Es gibt einen Boss gegner der mehr bzw. Komplexere Angriffsmuster hat

### Win Condition
Wenn der Boss NPC besiegt worde ist die Session erfolgreich zu ende.

### Lose Condition
Wenn die Basis des Spielers zerstört wurde ist das Spiel zuende.

