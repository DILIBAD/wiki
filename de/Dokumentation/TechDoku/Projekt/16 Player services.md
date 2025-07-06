# Player Services

## TLDR

Dieses Teilsystem bündelt alle Services rund um Spieler-Management: lokale Spielerreferenz, Spielerbasen-Verwaltung und Bereitstellung spielbarer Klassen.

## Zweck / Ziel

Der „Player Services“-Topic fasst drei Hauptaufgaben zusammen:

- **Lokaler Spieler und Session-Management**: Hält die Referenz auf den eigenen Spieler (`LocalPlayer`) bereit und verwaltet die Liste aller Spieler in Client- und Server-Kontext.
    
- **Spielerbasis-System**: Sorgt serverseitig für das Spawnen und Updaten von Basisgebäuden, definiert Spawn-Ziele für NPC-Wellen und behandelt Game-Over- bzw. Boss-Kill-Logik.
    
- **Klassen-Auswahl-Service**: Lädt und cached alle verfügbaren Spielerklassen aus Konfigurations-Assets, damit UI und Gameplay-Systeme sie einfach abrufen können.
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **Netzwerk & Session**:
        
        - `IGameNetworkService<IPlayerEntity>` für Verbindungs-Events
            
        - Internes Event-System für `OnPlayerConnected`/`OnPlayerDisconnected`
            
    - **Basis- und Gebäude-Management**:
        
        - `IBuildingEntity` / `ICombatBuildingEntity` für Gebäude-Health- und Tick-Logik
            
        - `ServiceLocator.SpawnService` & `ServiceLocator.GameLoopService`
            
        - Gruppierte UniTask-Loops nach `ITickableComponent.TickRate`
            
    - **Klassen-Konfiguration**:
        
        - `PlayerClassConfigTemplate` (ScriptableObject mit Templates)
            
        - `ICombatEntityTemplate<ICombatEntity>`-Interface
            
- **Ablauf und Interaktionen**
    
    1. **WarmUp/Start** lädt in allen Services ihre Daten:
        
        - Session-Service registriert lokale und entfernte Spieler.
            
        - Class-Selection-Service cached das erste Config-Asset.
            
        - Base-Service startet asynchronen Tick-Loop für Kampfgebäude.
            
    2. **Client vs. Server**:
        
        - Client: `LocalPlayer`-Property liefert das eigene Entity, `AllPlayers` alle entfernten.
            
        - Server: `AllPlayers` enthält alle verbundenen Spieler; `PlayerBaseService.Core` dient als Zielpunkt für NPC-Spawns.
            
    3. **Event-Flows**:
        
        - Spieler verbinden/trennen → Session-Service feuert Events → UI- und Gameplay-Systeme reagieren.
            
        - Gebäude spawnen und sterben → Base-Service registriert Health-Events → löst Game-Over oder Boss-Kill aus.
            
- **Architektur- und Designentscheidungen**
    
    - **Lose Kopplung** über Service-Locator und Event-Bus
        
    - **Gruppiertes Ticking** für Performance bei variierenden Tick-Raten
        
    - **Lazy-Loading & Caching** von ScriptableObject-Assets zur Minimierung von Ladezeiten
        
- **Reflektion: Überlegungen & Trade-offs**
    
    - Einfache Datentypen (Listen, ScriptableObjects) sind leicht handhabbar, skalieren aber unter sehr großen Sessions oder vielen Konfigurationen nur bedingt.
        
    - UniTask erleichtert asynchrone Loops, erfordert aber sorgfältiges Cancel-Management.
        
    - Aktuell keine Fallback-Mechanismen oder Hot-Reload für Klassen-Configs.
        


## Tests

1. **LocalPlayer-Referenz**
    
    - Session starten, eigenen Spieler erzeugen → `LocalPlayer` ist gesetzt und `DoesLocalPlayerExist` true.
        
2. **AllPlayers auf Client**
    
    - Mehrere Clients verbinden → `AllPlayers` auf einem Client liefert nur entfernte Spieler.
        
3. **AllPlayers auf Server**
    
    - Auf Server Spieler registrieren und abmelden → `AllPlayers` spiegelt stets den aktuellen Stand wider.
        
4. **Basis-Spawning & Core-Logik**
    
    - Core-Basis via `SERVER_SpawnBuilding` setzen → bei Zerstörung Game-Over auslösen.
        
    - NPC-Wellen initialisieren → prüfen, dass Ziel auf `PlayerBaseService.Core.Position` liegt.
        
5. **Kampfgebäude-Ticking**
    
    - Gebäude mit Tick-Components spawnen → `Tick()` wird in korrekten Intervallen ausgeführt; nach Tod keine Aufrufe mehr.
        
6. **Boss-Kill-Flow**
    
    - Boss-Gebäude spawnen, töten → `GameLoopService.SERVER_BossKilled()` wird aufgerufen.
        
7. **Klassen-Konfiguration laden**
    
    - Mehrere `PlayerClassConfigTemplate`-Assets vorhalten → `AllClasses` liefert Einträge des ersten Assets.
        
    - Leeres Config-Asset testen → `AllClasses` ist leer, keine Exception.
        
8. **UI-Integration**
    
    - Charakterauswahl-UI bindet an Service → alle Klassen werden korrekt angezeigt.
        

## Offene Punkte / Probleme

- **Duplikate & Namenskonflikte**: Keine Deduplizierung bei gleichnamigen Spielern oder Klassen.
    
- **Fehlerbehandlung**: Ungültige `UnregisterPlayer`-Aufrufe, fehlende `IBuildingEntity`-Komponenten führen zu Warnungen oder Exceptions ohne Recovery.
    
- **Skalierung**: Listen-basierte Datenhaltung und einfache Gruppierung können bei sehr großen Spielerzahlen oder vielen Tick-Raten Limits erreichen.
    
- **Hot-Reload**: Kein Mechanismus, um Klassen-Configs im laufenden Betrieb im Editor neu zu laden.
    
- **Konfigurationsvielfalt**: Auswahl immer nur des ersten Config-Assets, keine Profil- oder Szenario-Unterscheidung.