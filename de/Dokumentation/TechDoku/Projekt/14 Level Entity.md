
# TLDR

Automatisierte, kategoriebasierte Initialisierung, Verwaltung und Netzwerk-Interaktion von Leveln zur Reduktion manueller Konfigurationszeit.

## Zweck / Ziel

Die `LevelEntity` wurde entwickelt, um den bisherigen, zeitaufwändigen Editor-Window-Workflow abzulösen und Level-Konfigurationen per Dropdown-Kategorien zu standardisieren. Dadurch entfällt die wiederholte Zuweisung von Kategorien pro Zone, Setup-Fehler werden minimiert und die Setup-Dauer drastisch verkürzt.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - Partial-Klassen: Detection , LifeCycle , MapLink , SpawnHandling
        
    - `SpawnCategory` (Editor-Dropdown mit statischen Schlüsseln aus `LevelLocationKeys`)
        
    - `WorldConfigTemplate` / `FactionConfigTemplate` zur Konfigurationsübergabe
        
    - `ServiceLocator` für Spawn- und NPC-Services
        
    - Mirror-Komponenten: `NetworkBehaviour`, `netIdentity.visibility`, `NetworkStartPosition`
        
    - Unity-Komponenten: `Collider2D`, `AudioZone2D`
        
- **Ablauf und Interaktionen**
    
    1. **Server-Setup**: Per `SERVER_SetupLevel` werden Welt- und Fraktionskonfigurationen übergeben und `InitialSetup` aufgerufen, das alle ausgewählten Kategorien via `SetupCategory` verarbeitet.
        
    2. **Kategorie-Handling**:
        
        - `SetupAsZone` (mit Collider) vs. `SetupAsPosition` (ohne Collider) erzeugen initiale Spawn-Gruppen.
            
        - Spawns werden in Dictionaries je Kategorie getrackt.
            
    3. **Spawn-Weiterbetrieb**:
        
        - Über `UseZone` und `UsePosition` können respawn-Mechanismen und Wellen-Spawns (`TriggerWaveSpawn`) angestoßen werden.
            
        - `RespawnCategory` erlaubt dynamisches Auffüllen einzelner Kategorien.
            
    4. **Map-Verknüpfung**:
        
        - Methoden zum Hinzufügen/Entfernen von `MapConnection` und Abfrage freier Enter/Exit-Punkte (`HasFreeMapEnterConnection`, `GetRandomExitLocation`).
            
    5. **Laufzeit-Detektion**:
        
        - On-Client: Audio-Zonen-Tracking erfolgt bei Betreten der Map.
            
        - On-Server: Verwaltung aktiver Spieler, Sichtbarkeit per `Visibility.ForceShown/Hidden`.
            
    6. **Lebenszyklus & Disposal**:
        
        - `Dispose` entleert alle Spawn-Gruppen, entfernt Portal-Instanzen und räumt Dictionary-Einträge auf.
            
- **Architektur- oder Designentscheidungen**
    
    - **Partial Classes**: Trennung nach Verantwortlichkeit (Detection, Lifecycle, MapLink, SpawnHandling) für bessere Übersichtlichkeit.
        
    - **Stateless Dropdown-Kategorien**: Vermeidung von Tippfehlern und Duplikaten dank statischer Schlüssel.
        
    - **Collider- vs. Positions-Logik**: Einheitliches Handling von Zonen- und Punkt-Spawns basierend auf Präsenz eines `Collider2D`.
        
    - **Dictionary-Trennung**: Kategorisiert Spawns, um gezielte Zugriffe und Disposal zu ermöglichen.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Aktuell bestehen noch viele Code-Duplikate zwischen Setup- und Use-Methoden sowie zwischen Zone und Position.
        
    - Der monolithische Umfang erschwert Wartbarkeit; ein weiterer Refactor sollte zentrale Spawn-Strategien extrahieren und Wiederholungen reduzieren.
        
    - Die Validierung in dieser Version zeigte signifikante Performancesteigerung, erlaubt aber gezielte Identifikation von Refactor-Punkte.
        


## Tests

1. **Inspector-Konfiguration**
    
    - LevelEntity-Komponente auf GameObject hinzufügen.
        
    - Kategorien-Dropdown prüfen: nur gültige `LevelLocationKeys`-Einträge sichtbar.
        
2. **Initial-Setup**
    
    - Play-Mode starten.
        
    - Für jede gesetzte Kategorie verifizieren, dass Spawn-Gruppen korrekt erzeugt und im Dictionary abgelegt werden.
        
3. **Spawn-Verhalten**
    
    - Zonen-Spawn (mit Collider): Mehrfachspawns innerhalb der Collider-Fläche.
        
    - Positions-Spawn (ohne Collider): Einzelner Spawn auf exakter Transform-Position.
        
    - `TriggerWaveSpawn` und `RespawnCategory` via Netzwerk-Console aufrufen und auf korrekte Anzahl prüfen.
        
4. **MapLink-Verknüpfung**
    
    - `AddMapConnection`/`RemoveMapConnection` testen.
        
    - `HasFreeMapEnterConnection`/`GetRandomExitLocation` mit unterschiedlichen MapConnection-Zuständen validieren.
        
5. **Detektion & Sichtbarkeit**
    
    - Spieler betritt/verlässt Map-Area: Sichtbarkeit wechselt, AudioZones tracken korrekt.
        
6. **Disposal**
    
    - `Dispose` aufrufen und sicherstellen, dass alle Spawn-Gruppen geleert und Portale despawnt werden.
        
7. **Performance**
    
    - Setup-Dauer messen: Ziel < 10 Minuten vs. vorher ~ 1 Stunde.
        
8. **Edge Cases**
    
    - Fehlende Parent-Transforms → Konsolen-Warnungen.
        
    - Leere Kategorienliste → kein Setup, keine Fehler.
        
    - Polygon-Collider → Spawns berücksichtigen komplexe Flächen.
        

## Offene Punkte / Probleme

- **Refactor-Bedarf**: Hohe Code-Duplikation, insbesondere in `Setup*` vs. `Use*`.
    
- **Event-Handling**: `OnMapEmpty` derzeit deaktiviert, muss Wieder-Aktivierung und Tests erhalten.
    
- **Fehlerbehandlung**: Fehlende Null-Prüfungen bei ServiceLocator-Aufrufen können zu Laufzeitfehlern führen.
    
- **Zukünftige Anforderungen**: Einführung von Quest- und Coop-Puzzle-Features kann neue Kategorie-Typen und Spawn-Logiken erfordern.
    
- **Performance**: Bei sehr vielen Kategorien/Zonen könnten zusätzliche Optimierungen notwendig sein.