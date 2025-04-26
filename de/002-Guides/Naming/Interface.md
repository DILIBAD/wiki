---
title: Namenskonvention Interfaces
description: 
published: true
date: 2025-04-22T17:59:43.416Z
tags: 
editor: markdown
dateCreated: 2025-04-22T17:52:11.856Z
---

# 📊 Entscheidungsmatrix zur Interface-Namenskonvention

Diese Matrix hilft dir dabei, den **richtigen Namenspräfix für ein Interface** zu bestimmen – abhängig von Zweck, Verhalten und Datenfluss.

---
# 🎯 Interface-Namenskonvention – TL;DR Übersicht

| Format          | Zweck / Bedeutung                                           | Verwendet für …                          | Abgrenzung zu …                   |
|------------------|-------------------------------------------------------------|------------------------------------------|-----------------------------------|
| `ICanX`          | Objekt **kann** etwas tun, Fähigkeit oder Eigenschaft       | Zustandsprüfung, Logik, KI               | ❌ `IHandleX` (führt aktiv aus)   |
| `IHandleX`       | Objekt **verarbeitet** aktiv etwas                          | Input, Payloads, Ereignisse              | ❌ `ICanX` (nur Fähigkeit)         |
| `IControlX`      | Objekt **steuert gezielt** andere oder Verhalten            | AI-Controller, Gameflow-Logik            | ❌ `IHandleX` (nicht steuernd)     |
| `IXyzData`       | Strukturierte, **statische Daten** (z. B. ScriptableObject) | ItemDefinitionen, Stats                  | ❌ `IXyzInfo`, `IXyzPayload`       |
| `IXyzInfo`       | **Instanzzustand** eines Objekts                            | Zustand eines Items zur Laufzeit         | ❌ `IXyzData` (kein Zustand)       |
| `IXyzPayload`    | Temporäre **Übergabestruktur** zur Verarbeitung             | Schadensübergabe, Interaktionen          | ❌ `IXyzInfo` (passive Info)       |
| `IHasX`          | Objekt **besitzt** eine Komponente oder Eigenschaft direkt  | Inventory, Faction, Health               | ❌ `IProvideX` (könnte extern sein)|
| `IProvideX`      | Objekt **stellt X bereit**, aber besitzt es evtl. nicht     | Zugriff auf Blackboard, Config           | ❌ `IHasX`, `IReferenceX`          |
| `IReferenceX`    | Objekt **verweist** auf X, ohne Zugriff oder Ownership      | Item → ItemData, via ID                  | ❌ `IHasX`, `IProvideX`            |
| `IOnX`           | Reagiert bei einem **Event-Zeitpunkt**                      | Lebenszyklus, Spielereignisse            | ❌ `IHandleX` (unspezifisch)       |
| `IXyzService`    | **Funktionaler Service** (z. B. globaler Zugriff, Logik)     | Crafting, Upgrade, ResourceService       | ❌ `IHandleX` (nur Einzelverhalten)|
| `IXyzFactory`    | Erzeugt/instanziert Objekte oder Daten                      | ItemBuilder, PayloadCreator              | ❌ `IHandleX`                      |
| `IXyz`           | **Eigenständiges System oder Struktur** mit API & Zustand   | Blackboard, StateMachine, Inventory      | ❌ `IProvideX`, `IXyzData`         |

> 📌 Tipp: Suche nach **dem Zweck deines Interfaces**, nicht nach der Funktion deiner Klasse.

---

---

## ✅ Matrix: Welcher Interface-Typ passt?

| **Frage**                                                                 | **Wenn JA →**                       | **Verwende Interface-Format** | **Nicht verwenden, weil …**                        |
|---------------------------------------------------------------------------|-------------------------------------|-------------------------------|----------------------------------------------------|
| Führt das Objekt **eine Aktion aktiv aus**, wenn ein bestimmtes Ereignis eintritt? | z. B. verarbeitet `Input`, `Damage` | `IHandleX`                    | ❌ `ICanX`: drückt nur Fähigkeit, keine Aktivität aus |
| Steuert das Objekt ein **Subsystem oder Verhalten anderer Komponenten**? | z. B. gibt Befehle, setzt Verhalten | `IControlX`                   | ❌ `IHandleX`: wäre zu reaktiv, nicht aktiv steuernd |
| Gibt das Objekt an, ob **etwas grundsätzlich möglich ist**?              | z. B. kann geheilt werden           | `ICanX`                       | ❌ `IHandleX`: wäre irreführend, keine Ausführung     |
| **Stellt das Interface strukturierte Daten** über ein Objekt bereit?     | z. B. ItemData, DamageData          | `IXyzData`                    | ❌ `IXyzInfo`: beschreibt Instanzzustand, nicht Definition |
| **Beschreibt das Interface den Zustand einer Instanz zur Laufzeit**?     | z. B. Inventargegenstand aktiv      | `IXyzInfo`                    | ❌ `IXyzData`: wäre statisch, nicht instanzspezifisch |
| **Enthält es temporäre Daten zur Übergabe / Verarbeitung**?              | z. B. ein Angriff soll durchgeführt werden | `IXyzPayload`         | ❌ `IXyzInfo`: beschreibt Ergebnis, nicht Handlung     |
| Verfügt das Objekt **selbst über X**?                                     | z. B. Inventar-Komponente besitzt Daten | `IHasX`                  | ❌ `IProvideX`: zu ungenau, könnte extern sein         |
| Stellt das Objekt **Zugriff auf X bereit**, ohne es selbst zu besitzen?  | z. B. AI-Modul gibt Blackboard weiter | `IProvideX`              | ❌ `IHasX`: impliziert internen Besitz, wäre falsch   |
| Verweist das Objekt **auf ein anderes Objekt** (aber besitzt es nicht)?  | z. B. Inventory → ItemData          | `IReferenceX`                | ❌ `IHasX`: würde Besitz implizieren                  |
| Wird das Interface **bei einem Spielereignis** getriggert?               | z. B. `OnWaveStart`, `AfterDamage`  | `IOnX`, `IAfterX`            | ❌ `IHandleX`: ist allgemeiner, nicht ereignisgebunden |
| Gehört das Interface zu einem **Service, Factory oder Manager**?         | z. B. Spawn, Upgrade, Craft         | `IXyzService`, `IXyzFactory` | ❌ `IHandleX`: zu spezifisch, Service ist breiter     |
| Handelt es sich um ein **eigenständiges System/Struktur mit Zustand & Verhalten**? | z. B. Blackboard, Inventory         | `IXyz`                       | ❌ `IProvideX`: liefert nur Zugriff, ist aber nicht das System selbst |

---

## 🧠 Bedeutung und Abgrenzung pro Namensformat

### 🔧 `ICanX` – Fähigkeit

- **Verwendung**: Objekt kann unter bestimmten Bedingungen etwas tun.
- **NICHT verwenden**, wenn:
  - Das Verhalten tatsächlich **ausgeführt** wird → `IHandleX`
  - Das Objekt andere **steuert** → `IControlX`

---

### 🔁 `IHandleX` – Verarbeitung

- **Verwendung**: Objekt führt Verhalten aktiv aus.
- **NICHT verwenden**, wenn:
  - Nur geprüft wird, ob etwas möglich ist → `ICanX`
  - Das Verhalten aktiv zentral **gesteuert** wird → `IControlX`

---

### 🧭 `IControlX` – Steuerung

- **Verwendung**: Objekt kontrolliert andere Einheiten, Systeme oder Abläufe.
- **NICHT verwenden**, wenn:
  - Objekt nur auf Eingaben oder Events **reagiert** → `IHandleX`
  - Nur Eigenschaften definiert werden → `ICanX`

---

### 📦 `IXyzData` – Strukturierte Daten

- **Verwendung**: Z. B. Item-Blueprints, Skill-Werte, Statuswerte
- **NICHT verwenden**, wenn:
  - Der Zustand einer konkreten **Instanz** beschrieben wird → `IXyzInfo`
  - Eine Aktion **ausgelöst oder transportiert** wird → `IXyzPayload`

---

### 🔍 `IXyzInfo` – Laufzeitinformation

- **Verwendung**: Instanzspezifisch, z. B. aktuelle Ausrüstung
- **NICHT verwenden**, wenn:
  - Es sich um allgemeine Daten oder Definitionen handelt → `IXyzData`
  - Es eine Übergabestruktur ist → `IXyzPayload`

---

### 📤 `IXyzPayload` – Datenweitergabe

- **Verwendung**: Dient der Weitergabe/Ausführung einer Aktion (Angriff, Reparatur, etc.)
- **NICHT verwenden**, wenn:
  - Zustand/Nachwirkungen dargestellt werden sollen → `IXyzInfo`
  - Nur Grunddaten gespeichert werden → `IXyzData`

---

### 💾 `IHasX` – Besitz

- **Verwendung**: Objekt besitzt Daten, Komponenten oder Werte intern.
- **NICHT verwenden**, wenn:
  - Zugriff nur weitergegeben wird → `IProvideX`
  - Es sich um extern referenzierte Daten handelt → `IReferenceX`

---

### 📡 `IProvideX` – Zugriff (ohne Besitz)

- **Verwendung**: Gibt Zugriff auf Objekt, Komponente, Zustand, etc.
- **NICHT verwenden**, wenn:
  - Objekt selbst intern über die Daten verfügt → `IHasX`
  - Objekt nur referenziert und keine Funktionalität bietet → `IReferenceX`

---

### 🧭 `IReferenceX` – Objektverweis

- **Verwendung**: Objekt verweist auf eine andere Entität, z. B. via ID
- **NICHT verwenden**, wenn:
  - Zugriff oder Ownership impliziert ist → `IHasX`, `IProvideX`

---

### 🎮 `IOnX`, `IAfterX`, `IBeforeX` – Ereignisse

- **Verwendung**: Reaktion auf Lebenszyklusereignisse
- **NICHT verwenden**, wenn:
  - Allgemeine Reaktion auf beliebige Payloads erfolgt → `IHandleX`

---

### 🛠 `IXyzService`, `IXyzFactory`, `IXyzManager` – Systeme

- **Verwendung**: Größere Services, Lifecycle-Verwaltung, Objektgenerierung
- **NICHT verwenden**, wenn:
  - Verhalten nur auf eine Aktion begrenzt ist → `IHandleX`

---
## 🧩 Zusatzregel: `IXyz` für systemeigene Objekte

### Beispiele:  
- `IBlackboard<TKey>`  
- `IInventory`  
- `IStateMachine<TState>`  

Diese Interfaces:
- besitzen **internen Zustand**
- bieten eine **API für Zugriff & Manipulation**
- sind **zentral für Logik** wie AI, UI, Gameplay-Systeme

Verwende:
- `IProvideXyz` → wenn ein anderes Objekt auf diese Struktur zugreift
- `IXyz` → wenn das Objekt **selbst** das System ist

---

## ✅ Quick Reference Tabelle

| Zweck                             | Verwenden    | Nicht verwenden                |
|----------------------------------|--------------|--------------------------------|
| Fähigkeit prüfen                  | `ICanX`      | `IHandleX`, `IControlX`        |
| Verarbeitung von Aktionen         | `IHandleX`   | `ICanX`, `IControlX`           |
| Steuerung anderer Entities        | `IControlX`  | `IHandleX`, `ICanX`            |
| Objekt-/Blueprint-Daten           | `IXyzData`   | `IXyzInfo`, `IXyzPayload`      |
| Laufzeit-Instanzdaten             | `IXyzInfo`   | `IXyzData`, `IXyzPayload`      |
| Datenübergabe für Aktionen        | `IXyzPayload`| `IXyzInfo`, `IXyzData`         |
| Objekt besitzt X                  | `IHasX`      | `IProvideX`, `IReferenceX`     |
| Objekt stellt Zugriff auf X bereit| `IProvideX`  | `IHasX`, `IReferenceX`         |
| Objekt verweist auf anderes       | `IReferenceX`| `IHasX`, `IProvideX`           |
| Reaktion auf Events               | `IOnX`       | `IHandleX`                     |
| Services / zentrale Systeme       | `IXyzService`, `IXyzFactory` | `IHandleX`          |
| Zentrale Daten-/Laufzeitstruktur  | `IXyz`             | `IProvideX`, `IXyzData`        |

