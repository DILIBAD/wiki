# HealthComponent

# TLDR

Verwaltet Lebenspunkte, Todestatus und optionale Regeneration mit Netzwerk-Synchronisation und Ereignissen.

## Zweck / Ziel

Diese Klasse regelt die Grundfunktionen von Health:

- Speicherung von aktueller und maximaler Gesundheit
    
- Erkennung von Tod und Auslösen eines OnDeath-Events
    
- Automatische Regeneration nach einstellbarer Verzögerung
    
- Sichtbarkeit der Health-Anzeige steuerbar
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `IDamageableEntity` zur Verknüpfung mit DamageSystem
        
    - `UniTask` (Cysharp.Threading.Tasks) für asynchrone Regeneration
        
    - Mirror-SyncVar mit Hooks für nahtlose Client-Updates
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. `SERVER_Setup` initialisiert Max- und CurrentHealth
        
    2. `SERVER_Modify` passt Health an, feuert `OnHealthUpdated`, bricht laufende Regeneration ab
        
    3. Startet Nachverzögerung (`_regenDelay`) und anschließend Regeneration in Ticks (`_regenRate`, `_regenTickInterval`)
        
    4. Bei Tod (`Health <= 0` und `_diedAlready == false`) `OnDeath` und Flag setzen
        
    5. Respawn-Methoden setzen Health und `_diedAlready` zurück
        
- **Architektur- oder Designentscheidungen**
    
    - SyncVar-Hooks statt direkter Setter für konsistente Updates
        
    - CancellationTokenSource für präzises Abbrechen von Regenerations-Tasks
        
    - Keine Verwendung von `Update()`-Loops zur Ressourcenersparnis
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - - Saubere Trennung von Damage- und Regenerationslogik
            
    - - Netzwerksynchronisation ohne manuellen Sync-Aufwand
            
    - – Overhead durch UniTask und CancellationToken
        
    - – Potenzielle Speicherlecks bei mehrfacher CancellationSource-Erzeugung
        

## Screenshots / Diagramme

- **Zustandsmaschine**: Full → Damage → Delay → Regen → Full
    
- **Ablaufdiagramm**: `SERVER_Modify` → Hook → Event → (Regen-Setup) → `RegenerateAsync`
    

##  Tests Manuell oder durch Code ensurance

1. **Health-Clamp**
    
    - auf 0 und MaxHealth testen (z. B. Modify +200, –999)
        
2. **Todesevent**
    
    - Health auf 0 setzen → `OnDeath` genau einmal
        
    - Respawn → Health zurücksetzen und erneuter Tod möglich
        
3. **Regenerationsverhalten**
    
    - Schaden auslösen → keine Regeneration vor Delay
        
    - nach Delay → Health steigt schrittweise bis MaxHealth
        
    - erneuter Schaden während Delay → Delay zurückgesetzt
        
4. **SyncVar-Hooks**
    
    - Health-/MaxHealth-Änderungen auf Client prüfen (z. B. Entwicklungs-Build mit Logs)
        
5. **Sichtbarkeitsflag**
    
    - `SetHealthVisible(false/true)` → `IsHealthVisible` reflektiert Zustand
        

## Offene Punkte / Probleme

- CancellationTokenSource wird bei jedem Schaden neu erstellt, Dispose-Muster unvollständig
    
- Keine Obergrenze für Regenerationsrate definiert
    
- Kein Handling für Overheal (Heilung über MaxHealth)
    
- Fehlende Absicherung gegen Null-Referenzen bei Owner/StatsComponent