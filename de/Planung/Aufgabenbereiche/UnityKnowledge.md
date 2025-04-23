---
title: Unity Knowledge
description: 
published: true
date: 2025-04-23T09:48:35.441Z
tags: 
editor: markdown
dateCreated: 2025-04-19T17:53:30.220Z
---

## 📋 Nutzung der Tabelle

In der folgenden Tabelle können sich Teammitglieder bei den jeweiligen Unity-Bereichen eintragen. Ziel ist es:

- direkte Ansprechpartner für bestimmte Themen zu finden
- Transparenz darüber zu schaffen, wo Wissen im Team vorhanden ist
- gezielt Bereiche zu identifizieren, in die man sich einarbeiten kann oder Bedarf ist

> **Hinweis:** Einträge erfolgen bei Bedarf und dynamisch. Die Tabelle startet leer und wird vom Team ausgefüllt und gepflegt. Bei der Wahl der Features sollte sich an den Prioritäten Orientiert werden.

---

| Bereich | Experte oder auf dem Weg dahin | Für Rückfragen verfügbar | Nur Grundlagen | Grobes Verständnis |
| --- | --- | --- | --- | --- |
|  |    |    |     |     |
|  Animation & Animator   |   Alex  |     |     |     |
|     |     |     |     |     |
|     |     |     |     |     |
|     |     |     |     |     |
|     |     |     |     |     |

# 🧩 Priorisierte Unity-Bereiche

> Prioritäten:
> 
> - **(Erforderlich)**: Notwendig für Kernfunktionen
> - **(Neat to have)**: Ergänzen Gameplay/Qualität, aber optional
> - **(Dont touch me)**: Aktuell nicht relevant, ggf. später

---

## 🔴 Erforderlich

### 🌍 Networking & Multiplayer (Bei Interesse)

Netzwerkstruktur und Spiellogik für Mehrspieler mit Mirror. Synchronisation von Spielern, Zuständen und Objekten. Steuerung von Server- und Clientrollen, Verbindungsaufbau, Lobbys und Spielinstanzen.

### 🛠️ Asset-Erstellung & Import (Erforderlich)

Einbindung von 2D-Assets wie Sprites, Tilesets und Animationen aus externen Tools für die visuelle Gestaltung von Spielfiguren, Umgebungen und Interfaces. Nutzung von Asset Forge Repository und der Generierung von Assets mit [Pixelate](https://assetstore.unity.com/packages/tools/sprite-management/pixelate-pixel-art-converter-194727)
[Normal Maps für Objekte](https://www.youtube.com/watch?v=kpt7Ft5y8v4)
### 🎬 Animation & Animator (Erforderlich)

Bewegungsanimationen und Zustandsübergänge für Spielfiguren und NPCs, Umsetzung über Animator Controller mit Zustandsmaschinen.

Prüfung ob man Animation Events in dieser Art nutzen sollte: [Custom Animation Events](https://www.youtube.com/watch?v=XEDi7fUCQos)

Zu klären : States und Transition in Unity nutzen oder seperat über code steuern?

Oder soll erst mit dem Animator gearbeitet werden und später refactored werden um Komplexität in Sprint 1 niedrig zu halten ?
Resources:

[Unity Playables is Actually a Game-Changer - YouTube](https://www.youtube.com/watch?v=fQzKJO-0dS8)

[Build a Better Finite State Machine in Unity - YouTube](https://www.youtube.com/watch?v=NnH6ZK5jt7o)

[Don&#39;t use the Unity Animator, Use Code Instead! - Tutorial - YouTube](https://www.youtube.com/watch?v=I3_i-x9nCjs)

[Escaping Unity Animator HELL - YouTube](https://www.youtube.com/watch?v=nBkiSJ5z-hE)

### ⚙️ Physics (Erforderlich)

Grundlegende Bewegungs- und Kollisionssysteme basierend auf der 2D-Physikengine, inklusive Rigidbody2D, Collider2D und Triggern, Phsysics2D Klasse und Nutzung.

### 🧱 Tilemap & Grid-Systeme (Erforderlich)

Leveldesign basierend auf Raster-Systemen mit automatisch platzierbaren Kacheln (Tiles), inklusive Kollisionslayer für Interaktion.

### 🎮 Input-System (Input Action) (Erforderlich)

Erfassung und Verwaltung von Eingaben über Tastatur oder Controller. Unterstützt mehrere Geräte gleichzeitig und ermöglicht individuelles Rebinding.

Resources:

Im Sandbox Projekt genutzt.
[git-amend InputActions Tutorial] (https://www.youtube.com/watch?v=z5zShkCR0mg)
[Unity&#39;s NEW input system in 13 minutes - YouTube](https://www.youtube.com/watch?v=ONlMEZs9Rgw)

### 📱 UI & UX Design (Erforderlich)

Spieloberfläche für Menüs, Spielanzeigen (Health, Inventar), Interaktionen und Feedback. Wichtig für Navigation und Spielverständnis.

Zu Klären: Unitys Canvas System oder UIToolkit nutzen oder mit einem anfangen und zum anderen wechseln ? 
Resources:
[Build Procedural UI with Callbacks and Manipulators - YouTube](https://www.youtube.com/watch?v=MOiXqKFHAIs)

[UI Toolkit Runtime Data Binding and Logic - YouTube](https://www.youtube.com/watch?v=g2a4ZK8cEso)

### 🧾 ScriptableObjects (Erforderlich) (Kleiner Bereich)

Strukturierte Datenhaltung für Spielsysteme wie Fähigkeiten, Gegenstände oder Konfigurationen, ohne direkte Szeneabhängigkeit.

### 🔊 Audio (Erforderlich) (Überschaubar)

Einbindung von Soundeffekten, Musik und akustischem Feedback zur Verstärkung von Spieleraktionen und Atmosphäre.

### 🌲 Git / Versionskontrolle (Bei Interesse)

Nachverfolgung und Koordination von Projektänderungen im Team, Verwaltung von Branches und Konfliktvermeidung.

### ⚙️ CI/CD (Bei Interesse)

Automatisierung von Build-Prozessen, Tests und Bereitstellung neuer Versionen für Plattformen oder Testumgebungen.

---

## 🟡 Neat to have

### 🔥 Particle Effects (Neat to have)

Visuelle Effekte wie Explosionen, Trefferfeedback oder Magie zur Verstärkung von Spielereignissen.

### ✨ Visual Effects (VFX Graph / Shader Graph) (Neat to have)

Shaderbasierte Effekte zur optischen Aufwertung, z. B. Verzerrung, Glühen oder Energiewellen.

### 💡 Lighting (Neat to have)

Lichtquellen zur Darstellung von Stimmungen, Sichtweiten oder besonderen Spielsituationen (z. B. Dunkelheit, Lichtkegel).
[Normal Maps für Objekte](https://www.youtube.com/watch?v=kpt7Ft5y8v4)

### 📦 Addressables (Neat to have)

Dynamisches Laden und Verwalten von Assets zur Laufzeit für modulare Spielinhalte oder effizientere Ressourcennutzung.

### 🧪 Unit Tests (Neat to have)

Automatisierte Tests für Spiellogik wie z. B. Inventarsysteme, Netzwerkfunktionen oder Spielregeln zur Fehlervermeidung.

### 🔧 Profiling & Optimierung (Neat to have)

Leistungsanalyse für Laufzeitverhalten, Speicherverbrauch und Netzwerkbelastung. Identifikation von Engpässen.

### 💾 Saving & Serialization ((Neat to have))

Speicherung und Laden von Spielfortschritt oder Spielständen, inklusive serialisierter Datenstrukturen und Zustandstransfer bei Multiplayer.

### 📊 Analytics & Tracking (Neat to have)

Erfassung von Spieleraktionen und Verhalten zur Verbesserung von Balancing, UX und Spielverlaufsauswertung.

---

## ⚫ Dont touch me

### 🧰 Pixel Art Workflow & Pixel-Perfect Setup (Dont touch me)

Nur erforderlich bei strengen Pixeloptik-Anforderungen. Technische Absicherung für Auflösung und Darstellung.

### 🧠 Navigation & KI (Dont touch me)

Relevanz bei komplexen Gegnerbewegungen oder Pfadsuche – im einfachen Koop-Gameplay meist nicht kritisch.

### 🌐 Localization (Dont touch me)

Mehrsprachigkeit erst relevant bei internationaler Zielgruppe.

### 🎨 Post Processing (Dont touch me)

Hochwertige Bildnachbearbeitung, z. B. Tiefenunschärfe oder Bloom – oft überflüssig in klaren 2D-Stilen.

### 🎥 Cinemachine (Dont touch me)

Dynamische Kameraführung – selten notwendig bei fester oder einfacher 2D-Perspektive.

### 🖌️ Render Pipeline (Dont touch me)

URP wird vorausgesetzt, Anpassungen an der Renderstruktur sind jedoch meist nicht nötig.

### 🧩 DOTS (Dont touch me)

Performancetechnologie für massiv viele Objekte – nicht notwendig im typischen 2D-Multiplayer.