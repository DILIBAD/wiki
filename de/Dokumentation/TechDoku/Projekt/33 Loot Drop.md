# TLDR

Ermöglicht NPCs und Spielern, bei Tod oder Disconnect Gegenstände gemäß einer Wahrscheinlichkeits­tabelle im Spiel fallen zu lassen, um Exploits und Itemverlust zu vermeiden.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um

- **NPCs** beim Tod automatisch Items droppen zu lassen,
    
- **Spieler** bei Verbindungsabbruch oder Tod ihre gesamte Inventarausstattung fallen zu lassen,  
    und so zu verhindern, dass Items durch Exploits neutralisiert oder durch absichtliches Sterben umgangen werden.
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **LootProbabilityEntry**: Definiert einzelne Einträge mit Item-Referenz, Drop-Wahrscheinlichkeit und Menge.
        
    - **LootTable**: Aggregiert mehrere `LootProbabilityEntry`-Objekte und wählt per gewichteter Zufallsfunktion Einträge aus.
        
    - **LootDropHandler**: Führt auf dem Server Drop-Events aus und spawnt die gewählten Pickup-Objekte.
        
    - **ServiceLocator.SpawnService** & **AssetNexusService**: Zuständig für das tatsächliche Erzeugen der Pickup-Prefabs im Spiel über Mirror.
        
    - **ILocationPicker**: Bestimmt dynamisch den Spawn-Ort basierend auf Position und Spielweltbedingungen.
        
    - **Mirror.NetworkBehaviour**: Gewährleistet, dass Drops ausschließlich serverseitig ausgeführt werden.
        
- **Ablauf und Interaktionen**
    
    1. **Ereignis**: Entity stirbt oder Spieler trennt sich.
        
    2. **Server-Callback**: `LootDropHandler.SERVER_HandleDropEvent()` wird aufgerufen.
        
    3. **Zufallsauswahl**: `lootTable.GetEntry()` (oder mehrfach `GetEntries(amount)`) wählt Einträge basierend auf der konfigurierten Wahrscheinlichkeit aus.
        
    4. **Spawn**: Über `ServiceLocator.SpawnService.Server_Spawn(...)` werden die Pickup-Objekte an einer vom `ILocationPicker` geliefer­ten Position erzeugt.
        
- **Architektur- und Designentscheidungen**
    
    - **Datengetrieben**: Verwendung von Scriptable Objects (`LootProbabilityEntry`, `LootTable`) für einfache Balancing-Anpassungen ohne Codeänderung.
        
    - **Abstraktion**: Interfaces (`IProbabilityEntry`, `ILootTable`, `ICanDropItems`, `ILocationPicker`) für lose Kopplung und bessere Testbarkeit.
        
    - **Service Locator**: Zentrale Zugriffsschicht auf Netzwerk- und Asset-Services, um direkte Abhängigkeiten zu vermeiden.
        
    - **Serverautorität**: Sämtliche Drop-Logik läuft ausschließlich auf dem Server, um Cheating zu unterbinden.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Balancing zwischen Performance (viele gleichzeitige Drops) und Spielspaß (sichtbare Loot-Verteilung).
        
    - Eindeutigkeit von Spawn-Positionen vs. zufälliger Verteilung, um Blockierungen und Clipping zu vermeiden.
        
    - Aktuelle Implementierung unterstützt nur das Droppen einzelner Einträge; Mehrfachdrops müssen über `GetEntries(...)` explizit gehandhabt werden.
        
    - Unimplementierte Methode `SERVER_AddDrop` signalisiert Erweiterungsbedarf für dynamische Drops (z. B. besondere Gegenstände).
        


## Tests

1. **NPC-Drop**
    
    - NPC mit definiertem `LootTable` erstellen.
        
    - NPC töten → Überprüfen, ob Pickup-Objekt spawnte.
        
2. **Wahrscheinlichkeitsverteilung**
    
    - Dieselbe NPC-Konfiguration mehrfach (≥ 100 Durchläufe) töten.
        
    - Drop-Häufigkeiten gegen konfigurierte Wahrscheinlichkeiten validieren.
        
3. **Spieler-Disconnect**
    
    - Spieler Inventar füllen, Verbindung trennen.
        
    - Bestätigen, dass alle Inventar-Items als Pickups im Spiel erscheinen.
        
4. **Spieler-Tod**
    
    - Spieler absichtlich in Todesfall bringen (z. B. Sturz).
        
    - Überprüfen, dass keine Teleport-Exploits greifen und Items fallen.
        
5. **Netzwerkautorität**
    
    - Versuch, als Client Drop-Methoden aufzurufen → sicherstellen, dass keine Pickups generiert werden.
        
6. **Failover-Szenario**
    
    - LootTable leer oder ungültig konfigurierte `LootProbabilityEntry` → keine Exceptions, sondern saubere Fehlermeldung/Logging.
        

## Offene Punkte / Probleme

- `SERVER_AddDrop` ist noch nicht implementiert und limitiert dynamische Drop-Anforderungen.
    
- Keine Validierung, ob Summe der Wahrscheinlichkeiten = 100 %; fehlende Fehlerbehandlung bei falscher Konfiguration.
    
- Potenziell wachsende Anzahl an Drop-Objekten führt zu Performance-Einbußen bei hoher NPC-Dichte.
    
- Unschärfe bei `ILocationPicker`: Keine garantierte Kollisionsfreiheit beim Spawn—könnte zu Clipping führen.
    
- Künftige Erweiterung: Einschränkung von Maximal-Drops pro Zeitfenster, um Item-Spam zu verhindern.