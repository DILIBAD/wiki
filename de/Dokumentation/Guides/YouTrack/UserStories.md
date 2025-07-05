---
title: HowTo: Userstory
description: 
published: true
date: 2025-05-02T17:09:13.217Z
tags: 
editor: markdown
dateCreated: 2025-05-02T17:09:13.217Z
---

# Template

# User Story
**Als** Spieler  
**möchte ich** Münzen im Spiel aufsammeln  
**um** meinen Punktestand zu erhöhen.

## Scope (Abgrenzung)

### In Scope
- Kollisionserkennung zwischen Spieler und Münzen  
- Entfernen der Münze aus der Spielwelt nach Kontakt  
- Erhöhung des Punktestands um +1 pro Münze

### Out of Scope
- Verkauf oder Handel von Münzen  
- Sammeln spezieller Münztypen (Power-Up-Münzen)  
- Highscore-Liste oder Persistenz über Sessions

## Akzeptanzkriterien

### Muss-Kriterien (Minimalanforderungen)
1. **Münze aufsammeln**  
   - **Gegeben** der Spieler bewegt sich über eine Münze  
   - **Wenn** der Spieler die Münze berührt  
   - **Dann** verschwindet die Münze sofort aus der Spielwelt  
   - **Und** der Punktestand des Spielers erhöht sich um 1

2. **Keine Kollision**  
   - **Gegeben** der Spieler berührt keine Münze  
   - **Wenn** er über leeren Boden läuft  
   - **Dann** bleibt der Punktestand unverändert

### Kann-Kriterien (Optionale Anforderungen)
1. **Sound-Feedback**  
   - **Wenn** der Spieler eine Münze aufsammelt  
   - **Dann** kann ein Münz-Sammel-Sound abgespielt werden

2. **Visuelles Feedback**  
   - **Wenn** die Münze eingesammelt wird  
   - **Dann** kann eine kurze Partikel-Animation angezeigt werden

---
```
# User Story
**Als** Spieler  
**möchte ich** Münzen im Spiel aufsammeln  
**um** meinen Punktestand zu erhöhen.

## Scope (Abgrenzung)

### In Scope
- Kollisionserkennung zwischen Spieler und Münzen  
- Entfernen der Münze aus der Spielwelt nach Kontakt  
- Erhöhung des Punktestands um +1 pro Münze

### Out of Scope
- Verkauf oder Handel von Münzen  
- Sammeln spezieller Münztypen (Power-Up-Münzen)  
- Highscore-Liste oder Persistenz über Sessions

## Akzeptanzkriterien

### Muss-Kriterien (Minimalanforderungen)
1. **Münze aufsammeln**  
   - **Gegeben** der Spieler bewegt sich über eine Münze  
   - **Wenn** der Spieler die Münze berührt  
   - **Dann** verschwindet die Münze sofort aus der Spielwelt  
   - **Und** der Punktestand des Spielers erhöht sich um 1

2. **Keine Kollision**  
   - **Gegeben** der Spieler berührt keine Münze  
   - **Wenn** er über leeren Boden läuft  
   - **Dann** bleibt der Punktestand unverändert

### Kann-Kriterien (Optionale Anforderungen)
1. **Sound-Feedback**  
   - **Wenn** der Spieler eine Münze aufsammelt  
   - **Dann** kann ein Münz-Sammel-Sound abgespielt werden

2. **Visuelles Feedback**  
   - **Wenn** die Münze eingesammelt wird  
   - **Dann** kann eine kurze Partikel-Animation angezeigt werden

```


