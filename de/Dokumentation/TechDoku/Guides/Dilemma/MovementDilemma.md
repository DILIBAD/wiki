---
title: Movement Dilemma
description: 
published: true
date: 2025-04-19T14:27:30.037Z
tags: 
editor: markdown
dateCreated: 2025-04-19T14:27:30.037Z
---

# 🧭 Bewegungsmethodik in Unity 2D – Wann Transform, Rigidbody2D oder Pathfinding?

> **TL;DR:**  
> Verwende **Pathfinding**, wenn Navigation um Hindernisse erforderlich ist.  
> Nutze **Rigidbody2D**, wenn Bewegung mit Kollision und Physik interagieren soll.  
> Setze **Transform-Movement** ein, wenn einfache, direkte Bewegung ohne Physik genügt.

---

## ❓ Die Frage

**Welche Bewegungsmethode ist in Unity 2D wann sinnvoll?**  
– Die Wahl hat Auswirkungen auf Performance, Genauigkeit, Komplexität und Verhalten bei Kollisionen.

---

## ⚙️ Überblick der Methoden

### 🔄 `Transform.position` / `Transform.Translate`

- Direkte Änderung der Position.
- **Kein physikalischer Einfluss** – lässt bewegungen durch Collider zu
- Sehr performant, aber ungeeignet für physikabhängige Objekte.

**Typische Einsätze:**

- UI-Elemente
- Effekte, z. B. Gleiter oder Plattformen
- Einfache Gegner- oder Kamerabewegung
- Einfache Bewegung auf geraden Linien bspw. über Wegpunkte
- Kein Ausweichen nötig

---

### ⚽ `Rigidbody2D` (z. B. mit `velocity`, `MovePosition`, `AddForce`)

- Nutzt Unitys Physiksystem.
- Löst korrekt Trigger- und Collision-Events aus.
- Kompatibel mit Gravitation, Reibung, Massen etc.

**Typische Einsätze:**

- Spielerbewegung
- Interaktive Gegnerbewegung
- Physikpuzzle oder Plattformmechanik

---

### 🧠 Pathfinding

- Berechnet einen Pfad von A nach B unter Berücksichtigung von Hindernissen.
- Nutzt typischerweise ein Grid oder ein NavMesh (auch 2D).
- Oft in Kombination mit Rigidbody2D oder MovePosition genutzt.

**Typische Einsätze:**

- Gegner-KI in Labyrinthen
- NPCs mit Navigation
- Einheiten in Echtzeitstrategie
- Spielerbewegung

---

## 🧭 Entscheidungshilfe

| Kriterium | Transform | Rigidbody2D | Pathfinding |
| --- | --- | --- | --- |
| Direkte, einfache Bewegung | ✅ Ja | ⚠️ Möglich | ❌ Nein |
| Kollisionserkennung nötig | ❌ Nein | ✅ Ja | ⚠️ Nur in Kombination |
| Physik (Schwerkraft, Trägheit) | ❌ Nein | ✅ Ja | ❌ Nein |
| Hindernisse umgehen | ❌ Nein | ❌ Nein | ✅ Ja |
| Kontrolle über Geschwindigkeit/Kräfte | ❌ Nein | ✅ Ja | ⚠️ Pfadabhängig |
| Performance | ✅ Hoch | ⚠️ Mittel | ⚠️ Abhängig vom Setup |
| Komplexität | ✅ Gering | ⚠️ Mittel | ❌ Hoch |

---

## 🧪 Beispiele

### 1. Transform-Movement

```csharp
transform.position = Vector2.MoveTowards(transform.position, targetPosition, speed * Time.deltaTime);
```

---

### 2. Rigidbody2D mit `velocity`

```csharp
rigidbody2D.velocity = speed * direction ;
```

---

### 3. Pathfinding + Bewegung (vereinfachtes Konzept)

```csharp
List<Vector2> path = Pathfinder.FindPath(currentPosition, targetPosition);
MoveAlongPath(path); // Bewegung z. B. mit MovePosition oder Interpolation
```

---

## 🧼 Fazit

Die Bewegungsmethode sollte bewusst gewählt werden:

- **Transform-Movement** für einfache, skriptgesteuerte Bewegungen ohne physikalische Anforderungen.
- **Rigidbody2D** für alles, was mit Kollision oder Physik reagiert.
- **Pathfinding**, wenn Navigation durch komplexe Räume erforderlich ist.

Eine Kombination ist oft sinnvoll – z. B. Pathfinding zur Zielermittlung + Rigidbody2D zur Bewegungsausführung.
Grundlegend werden im Projekt Interfaces für alle Bewegungsarten hinterlegt und genutzt. Eine Implementierung ist somit nur einmalig nötig.