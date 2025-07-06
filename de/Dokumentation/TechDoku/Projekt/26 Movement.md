# Navigation2D Movement

# TLDR

Implementierung der 2D-Bewegung über das Navigation2D-Asset (Unity 3D NavMesh) mit Mirror-Netzwerksynchronisation; projiziert 2D-Welt auf Y=0, unterstützt mehrere Level/NavMesh-Surfaces und entkoppelt über Interfaces für einfache Wartbarkeit und möglichen Refactor.

## Zweck / Ziel

Dieses Subsystem wurde entwickelt, um die Bewegung von Agents in einer 2D-Welt mit Unitys eingebautem 3D-NavMesh-System zu ermöglichen und gleichzeitig eine effiziente Netzwerksynchronisation über Mirror bereitzustellen. Es löst folgende Probleme:

- Nutzung eines bewährten, robusten NavMesh-Systems trotz 2D-Spielwelt
    
- Unterstützung mehrerer Level mit eigenständigen NavMesh-Surfaces
    
- Bandbreiten­schonende Synchronisation von Zielposition oder Geschwindigkeit
    
- Entkopplung der Bewegungskomponente mittels Interfaces für zukünftigen Austausch
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **NavMeshAgent2D** (2D-Ableitung von Unitys NavMeshAgent, führt Pfadfindung/Base-Movement durch)
        
    - **NetworkNavMeshAgent2D** (Mirror-NetworkBehaviour, SyncVar-basiertes Senden von Ziel/Velocity + Warp-RPC)
        
    - **ServerNavMeshMovementComponent2D** (vermittelt Eingaben, Commands/RPCs, implementiert `IMovementComponent`)
        
    - **NavMeshSurface** (Unity-Komponente, baut auf Level-Basis die NavMesh-Daten auf)
        
    - **Interfaces**: `IMovementComponent`, `IMovingCombatEntity`
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **Level-Initialisierung**
        
        - Pro Level wird ein eigener `NavMeshSurface` erstellt, der aus der 2D-Szene in Y=0 projiziert.
            
    2. **Server-Start**
        
        - `ServerNavMeshMovementComponent2D` aktiviert NavMeshAgent2D-Positionsupdate, initialisiert Referenzen.
            
    3. **Client-Start**
        
        - Lokaler Player setzt Agent-Update-Flag, aktiviert Netzwerkagenten.
            
    4. **Eingabe → Bewegung**
        
        - Client sendet per Command (`MoveImmediate` oder `MoveTo`) Richtung oder Ziel an Server.
            
        - Server setzt Agent-Velocity oder Destination auf Basis der Interfaces und Autorität.
            
    5. **Netzwerksynchronisation**
        
        - `NetworkNavMeshAgent2D.OnSerialize` sendet Position + Geschwindigkeit oder Ziel + StoppingDistance, nur wenn sich Werte geändert haben.
            
        - `OnDeserialize` wendet Bewegung an, führt bei Abweichung/Warp Notfall-Teleport durch.
            
- **Architektur- oder Designentscheidungen**
    
    - **Projektion 2D → 3D**: 2D-Positionen und Objekte werden in der XZ-Ebene (Y=0) gehandhabt, um Unitys 3D-System zu nutzen.
        
    - **Interface-Abstraktion**: Movement-Logik ist über `IMovementComponent` und `IMovingCombatEntity` gekapselt, um bei Bedarf ein reines 2D-Pathfinding-System (Custom) einzusetzen.
        
    - **Lazy-Sync**: Bandbreite gespart durch bedarfsorientiertes Senden (nur bei Destination- oder Velocity-Änderung).
        
    - **Warp-Mechanismus**: Rubberbanding-Erkennung durch Distanzmessung und RPC-Aufruf, um Clients notfalls auf Server-Position zurückzusetzen.
        
- **Reflektion: Wichtige Überlegungen und Trade-offs**
    
    - **Komplexität vs. Robustheit**: Nutzung von Unitys NavMesh minimiert Eigenaufwand, erhöht aber Komplexität (Projektion, mehrere Surfaces).
        
    - **Performance-Potential**: Der Umweg über 3D könnte wegoptimiert werden – reine 2D-Pfadfinding-Bibliothek wäre performanter und wartbarer.
        
    - **Netzwerk-Overhead vs. Konsistenz**: SyncVar-basierte Methode spart Bandbreite gegenüber ständigen Position-Updates, erfordert aber zusätzliche Logik für Warp/Fehlerfälle.
        
    - **Entkoppelung**: Interfaces ermöglichen, das Subsystem bei einem Refactor unkompliziert gegen eine reine 2D-Lösung auszutauschen.
        

## Tests

1. **Level-Laden & NavMeshSurface**
    
    - Jedes Level laden und prüfen, dass das NavMesh im Editor und Play-Mode sichtbar und begehbar ist.
        
2. **Click-Movement vs. WASD**
    
    - Auf Client Click unter verschiedenen Bedingungen (offenes Pfadnetz, Hindernisse) ausführen, prüfen, dass Ziel erreicht wird.
        
    - WASD-Steuerung testen, Geschwindigkeit und Richtung auf allen Clients synchron.
        
3. **Netzwerksynchronisation**
    
    - Zwei Clients verbinden, Player A bewegt sich, Player B sieht reibungslose Bewegung ohne Teleport-Artefakte.
        
4. **Warp/Rubberbanding**
    
    - Agent manuell weit versetzen, prüfen, ob Rubberband-Logik den Client korrekt auf Server-Position zurücksetzt.
        
5. **Autoritätswechel & Teleport**
    
    - Teleport-Aufruf vom Client und Server ausführen; Position bleibt auf allen Instanzen identisch.
        
6. **Deaktivierung & Rotation Prevention**
    
    - `Disable()` und `SetPreventRotation(true)` aufrufen und validieren, dass Agent weder bewegt noch rotiert.
        
7. **Mehrere Level-Wechsel**
    
    - Nach Szenenwechsel kontrollieren, dass beim neuen Level der korrekte NavMesh benutzt wird.
        

## Offene Punkte / Probleme

- **Performance-Overhead** durch 3D-Projektion und mehrfaches Baking der NavMeshSurface.
    
- **Fehlende reine 2D-Lösung**: Refactor zu spezialisierten 2D-NavMesh-Bibliotheken könnte Wartbarkeit und Geschwindigkeit steigern.
    
- **Race Conditions** beim gleichzeitigen Laden mehrerer NavMeshSurface-Instanzen in Start-Phasen.
    
- **Ungenutzte Warp-Calls**: Einige Teleport-Logiken sind auskommentiert, ggf. nochmal überprüfen und vollständig implementieren.
    
- **Scrollende Pfadfindung**: Bei sehr dynamischen Hindernissen derzeit keine Laufzeit-NavMesh-Updates, führt zu falsch berechneten Pfaden.