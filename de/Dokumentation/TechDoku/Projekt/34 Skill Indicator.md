# Skill Indicator Subsystem

# TLDR

Bietet netzwerksynchrone und lokale Skalierungs-Indikatoren für Skills und Effekte, die sowohl laufzeitdynamische Radius-Darstellungen als auch zeitlich gesteuerte Animations-Loops umfassen.

## Zweck / Ziel

Das Indicator Subsystem wurde entwickelt, um in einem Mirror-basierten Unity-Projekt zwei gängige Anzeigeanforderungen abzudecken:

1. **Skill-Radius-Darstellung**: Einfaches Abbilden eines veränderlichen Wirkungsradius im Spiel durch direkte Skalierung eines GameObject-Transforms.
    
2. **Zeitgesteuerte Loop-Animation**: Netzwerksynchron gesteuerte Skalierungsanimation (von 0 bis zur Ursprungsgröße) für beliebige Indikatoren, ausgelöst durch den Server und auf allen Clients gleichzeitig abgespielt.
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **Transform**: Basis aller visuellen Änderungen
        
    - **Mirror Networking**:
        
        - Server-Methode löst Client-RPC aus
            
        - `RpcTriggerIndicator` synchronisiert Animation auf allen Clients
            
    - **Cysharp.Threading.Tasks (UniTask)** & **CancellationToken**:
        
        - Frame-genaue, asynchrone Loop-Animation
            
        - Abbruchlogik bei Objektzerstörung oder Verbindungsabbruch
            
    - **AbilityManager / externe Logik**:
        
        - Übergibt Radiuswerte an den Skill-Radius-Indikator
            
    - **Subklassen-Pattern**:
        
        - `IndicatorLoopBase` als abstrakte Basis
            
        - `SimpleScaleIndicatorLoop` als konfigurierbarer Präfabrik-Baustein
            
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **Skill-Radius**
        
        - Externe Logik ruft `UpdateScale(radius)` auf
            
        - `localScale` des zugewiesenen Transforms wird direkt auf `(radius, radius, radius)` gesetzt
            
    2. **Loop-Animation**
        
        - Server löst `TriggerIndicator(duration)` aus
            
        - Mirror-RPC startet `ShowIndicatorAsync()` auf allen Clients
            
        - Asynchrone Loop setzt zu Beginn Scale auf 0 (je nach Achsenauswahl) und interpoliert über die Dauer zurück zur Ursprungs-Scale
            
        - Nach Ablauf wird die Original-Scale wiederhergestellt
            
- **Architektur- oder Designentscheidungen**
    
    - **Trennung Client/Server**: Animation läuft vollständig clientseitig, Server steuert nur den Start
        
    - **UniTask vs. Coroutine**: Besseres Handling von CancellationTokens und Integration in async/await-Flow
        
    - **Direktes Setzen vs. Tweening**:
        
        - Skill-Radius verzichtet auf Tween für maximale Performance
            
        - Loop-Animation nutzt Interpolation im UniTask-Loop
            
    - **Leichtgewichtige Subklassen**: Ermöglichen schnelle Konfiguration im Editor ohne neue Logik
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Clientseitige Ausführung** kann zu Abweichungen führen, wenn Clients stark ausgelastet sind
        
    - **Keine Überschreibungslogik**: Mehrfach-Trigger können sich überlagern oder Animationszustand inkonsistent lassen
        
    - **Kein integriertes Error-Handling** für fehlende Referenzen oder ungültige Werte
        
    - **Transform-Manipulation** kann mit anderen Komponenten kollidieren, wenn mehrere Skripte `localScale` ändern
        

## Screenshots / Diagramme

- **Inspector-Setup**:
    
    - Zuweisung des Indicator-Transforms und der Achsen-Flags
        
    - Referenz zum AbilityManager für Skill-Radius
        
- **Sequenzdiagramm**:
    
    ```
    Server       Client A       Client B
       |              |               |
       |---Trigger--->|               |
       |              |--RpcTrigger-->|
       |              |               |
       |              |--ShowLoopAsync()
       |              |               |
    ```
    
- **Transform-Hierarchie**: Darstellung der Child-Beziehung zwischen Skill-Objekt und Indikator
    

## Tests

1. **Skill-Radius**
    
    - Manuelle Änderung des Radius im Play-Mode und sofortige Verifikation der Skalierung
        
    - Grenzwerte: 0, negative Werte, sehr große Werte (>10)
        
2. **Loop-Animation**
    
    - Host als Server im Editor starten, Client verbinden
        
    - Trigger über Server-Konsole für verschiedene Dauern (0 s, 2,5 s, 15 s)
        
    - Überprüfung, dass auf allen Clients die Skalierung simultan und gleichmäßig abläuft
        
3. **Fehler- und Abbruchfälle**
    
    - Entfernen des Indicator-Transforms im Lauf → keine Exceptions
        
    - Verbindungsabbruch während Loop → sauberer Abbruch und Rücksetzung
        
    - Mehrfaches Auslösen in kurzen Abständen → Beobachtung von Überlagerungen
        
4. **Konfigurationsvalidierung**
    
    - Achsen-Flags einzeln an-/ausschalten und Wirkung prüfen
        
    - Fehlende Konfigurationen (null-Referenzen) → Warnungen im Log
        

## Offene Punkte / Probleme

- **Kein Vorzeit-Stopp**: Laufende Loop-Animation kann nicht programmgesteuert unterbrochen werden
    
- **Ungültige Werte**: Keine automatische Validierung negativer oder unrealistisch hoher Dauer- und Radiuswerte
    
- **Concurrency**: Überlagerung mehrerer Trigger infolge fehlender Queue- oder Priorisierungslogik
    
- **Erweiterbarkeit**: Aktuell keine Unterstützung für Farb-/Material-Changes oder Tween-Kurven bei Radius-Änderungen
    
- **Dokumentation**: `SimpleScaleIndicatorLoop` ist leer und sollte entweder mit Preset-Konfiguration bestückt oder entfernt werden