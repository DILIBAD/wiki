# Inventory Item Subsystem

# TLDR

Ein modulares ScriptableObject-basiertes System, das Item-Templates verwaltet, Laufzeit-Instanzen serialisiert und Inventar-Container mit Netzwerk-Synchronisation bereitstellt.

## Zweck / Ziel

Dieses Subsystem soll sämtliche Aspekte von Items in unserem Unity-Projekt abdecken:

- **Konfiguration** über ScriptableObjects für generische, nutzbare und tragbare Items,
    
- **Laufzeit-Repräsentation** einzelner Item-Instanzen mit Menge, Stack-Logik und Serialisierung,
    
- **Container-Management** mit Add/Remove/Query-Methoden, Events und Mirror-Synchronisation.
    

Es erlaubt neue Items ohne Code-Änderungen anzulegen, minimiert Laufzeit-Overhead durch schlanke Datenstrukturen und stellt konsistente Inventarzustände auf Server und Clients sicher.

## Umsetzung

### Relevante Kooperationspartner

- **ItemTemplate**: Basisklasse für alle konfigurierbaren Items, enthält Name, Beschreibung, Icon und MaxStackSize
    
- **WearableItemTemplate**: Erweitert ItemTemplate um Stat-Modifier, wendet beim Equip/Uneqip serverseitig Modifikatoren an
    
- **ItemCostTemplate** & **ItemTemplateMapper**: Definieren Ressourcen-Kosten via Arrays von Mappers (ItemTemplate + Amount)
    
- **ItemData**: Value-Struct, das per HashID auf das zugehörige Template verweist (lazy-loading)
    
- **ItemInstance**: Struct mit Item-ID, Menge, Stack-Logik (Increase/Decrease) und Prüfung auf Useable/Wearable
    
- **ItemInstanceInfoSerialize**: NetworkWriter/Reader-Extensions zur schlanken Serialisierung (nur ID & Amount)
    
- **ItemContainer**: NetworkBehaviour mit `SyncList<ItemInstance>`, Cmd/SERVER-Methoden, Events für UI-Bindings und Drop-Mechanik
    

### Ablauf und Interaktionen (Unity & Networking)

1. **Editor**: Items werden via `CreateAssetMenu` als ScriptableObject-Assets angelegt und konfiguriert.
    
2. **Initialisierung**: Beim Spielstart cached das ScriptableObject-Basis-Subsystem alle Templates in Dictionaries.
    
3. **Instanziierung**: Hinzufügen/Entfernen im Inventar erzeugt `ItemInstance`-Structs.
    
4. **Netzwerk**: Mirror nutzt `WriteItemInstanceInfo`/`ReadItemInstanceInfo` zur Übertragung, Client baut Instanz und verknüpft sie mit Template-Daten.
    
5. **Container-Operationen**: Client-Commands (`Cmd_AddById`, `Cmd_DropItem`) rufen serverseitige Methoden auf. Änderungen an `SyncList` feuern Events (`OnItemAdded`, `OnItemRemoved` etc.), die UI und Gameplay-Logik aktualisieren.
    
6. **Drop**: Auf Server spawned `ItemContainer` Pickup-Prefabs über den AssetNexusService und entfernt Instanzen aus dem Inventar.
    

### Architektur- oder Designentscheidungen

- **ScriptableObject-Caching** vermeidet teure Resources-Loads zur Laufzeit.
    
- **Interface-basiertes Design** (`IUseableItemData`, `IWearableItemData`, `IItemCostTemplate`) für einfache Erweiterbarkeit.
    
- **Value-Structs** (`ItemData`, `ItemInstance`) minimieren Heap-Allokationen unter Last.
    
- **Event-basiertes UI-Binding** über `SyncList`-Callbacks statt Polling.
    
- **Granulare Serialisierung** (nur ID & Amount) reduziert Netzwerk-Traffic.
    

### Reflektion: wichtige Überlegungen und Trade-offs

- **+ Wartbarkeit**: Designer können Items ohne Entwickler eingreifen konfigurieren.
    
- **+ Performance**: Schlanke Structs und minimalistisches Serialisieren.
    
- **– Kopplung**: Grafiken und Prefabs sind noch in ScriptableObjects referenziert, erschwert Live-Änderungen ohne Patches.
    
- **– Netzwerk-Bandbreite**: Viele SyncList-Änderungen bei großen Inventaren können zu Traffic-Spitzen führen.
    


## Tests

1. **ItemTemplate-Erstellung**
    
    - Asset anlegen, Felder füllen, Spielstart: Cache-Log auf vollständiges Laden prüfen.
        
2. **Wearable Equip/Uneqip**
    
    - Equip/Ausrüsten: `SERVER_Equip` aufrufen, Stat-Änderungen im Entity-Log verifizieren.
        
    - Unequip: `SERVER_Unequip`, Stat-Rollback prüfen.
        
3. **Serialisierung**
    
    - Zwei-Client-Setup: Item ins Inventar hinzufügen, ID & Amount im Netzwerkpaket validieren, Client baut Instanz korrekt auf.
        
4. **Container Operationen**
    
    - Stapel-Limit erreichen, weiteres Hinzufügen ablehnen (`CanAddById`).
        
    - Entfernen vs. vollständiges Leeren, `keepEmptySlots`-Flag testen.
        
    - Drop: `Cmd_DropItem` aus UI, Prefab-Spawn und Inventar-Update prüfen.
        
5. **Kosten-Check**
    
    - `ItemCostTemplate` mit mehreren `ItemTemplateMapper` konfigurieren, Crafting-Subsystem simulieren, Vergleich zu Inventar-Beständen.
        

## Offene Punkte / Probleme

- **Daten-/Asset-Entkopplung**: Grafiken und Prefabs sind direkt referenziert. Eine Migration zu ID-basierten Verweisen mit externem Dashboard würde Live-Service-Updates ohne Patches erlauben.
    
- **Exception-Sicherheit**: Ungültige IDs in `ItemData.data` führen zu `KeyNotFoundException`; Fallback-Mechanismus könnte Stabilität erhöhen.
    
- **Batched-Updates**: Für große Inventare wäre ein gebündeltes SyncList-Update sinnvoll, um Netzwerk-Traffic zu dämpfen.
    
- **Live-Service-Potential**: Extraktion von Daten/Assets und Generative KI-Integration könnte die Content-Erstellung automatisieren und Entwicklern Ressourcen sparen.