# Network-Cooldown Extensions

## TLDR

Ermöglicht die Berechnung der verbleibenden Abklingzeit und deren prozentualen Anteil basierend auf der Mirror-Netzwerkzeit. CooldownExtender

## Zweck / Ziel

Diese Erweiterungen wurden entwickelt, um in Skills (ISkillDataTemplate) jederzeit bequem abzufragen, wie viel Zeit bis zur nächsten Nutzung verbleibt und wie viel Prozent der Gesamt­abklingzeit noch aussteht, ohne dass jeder Aufrufer selbst die Netzwerkzeit berücksichtigen muss.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `ISkillDataTemplate` (Skill-Daten-Interface)
        
    - `NetworkTime.time` aus Mirror für konsistente Netzwerkzeit CooldownExtender
        
- **Ablauf und Interaktionen**
    
    1. Aufruf von `GetRemainingCooldown(nextUsage)` oder `GetRemainingCooldownPercentage(nextUsage)`
        
    2. Ermittlung der Differenz: `nextUsage − NetworkTime.time`
        
    3. Rückgabe des Restwerts bzw. Normierung auf `[0,1]`
        
- **Architektur- oder Designentscheidungen**
    
    - Nutzung einer statischen Erweiterungsklasse für losgelöste Wiederverwendbarkeit
        
    - Trennung von Skill-Logik und Zeitberechnung
        
    - Einsatz von `double` für Netzwerkgenauigkeit, `float` zur Anzeige
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Potenzielle Drift bei nicht-synchronisiertem Client-Clock
        
    - Grenzfälle: negative Zeiten werden auf 0 gekappt
        
    - Performance: einfacher Rechenaufwand, kann aber bei sehr hoher Aufrufrate zu Overhead führen
        

## Screenshots / Diagramme

- **Sequenzdiagramm**: Client → Skill UI → CooldownExtender → Mirror NetworkTime → UI-Update
    
- **Datenflussdiagramm**: SkillDataTemplate ▶︎ CooldownExtender ▶︎ Netzwerkzeit ▶︎ Prozentrechnung ▶︎ Anzeige
    

## Tests

1. **Initialisierung**: Skill mit definierter Cooldown starten, `nextUsage = NetworkTime.time + Cooldown`.
    
2. **Vor Ablauf**: nach 25 % der Cooldown-Dauer aufrufen, Prozentwert ≈ 0.75 prüfen.
    
3. **Nach Ablauf**: weit nach `nextUsage`, verbleibende Zeit = 0, Prozent = 0.
    
4. **Edge Cases**: Cooldown = 0 → Prozent immer 0.
    
5. **Netzwerk-Drift simulieren**: unterschiedliche `NetworkTime.time` zwischen Server/Client, Konsistenz sicherstellen.
    

## Offene Punkte / Probleme

- **Netzwerk-Zeitdrift** zwischen Clients kann zu falschen Werten führen.
    
- **Precision-Loss** bei sehr kleinen oder sehr großen Cooldown-Werten.
    
- **Abhängigkeit** von Mirror – bei Migration zu anderem Netzwerk-Toolkit Anpassung nötig.