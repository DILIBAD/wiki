# HandleInventory Component

# TLDR

Erweitert das generische `ItemContainer` um konfigurierbare Slot-Anzahl, prüft freie/vergebene Plätze, implementiert `ILootingEntity` für Loot-Interaktionen und stellt einfache Add/Remove-APIs bereit.

## Zweck / Ziel

Die `HandleInventoryComponent` kann an jede Entität angehängt werden, die lootbar sein soll (`ILootingEntity`). Sie verwaltet ein Inventar mit fester Slot-Anzahl und Typ-Restriktionen, beantwortet Fragen wie „Wie viele Plätze sind frei?“ und ermöglicht kontrolliertes Hinzufügen/Entfernen von Items.  
Durch die Implementierung von `ILootingEntity` werden Loot-Mechaniken wie zufälliges Befüllen, Loot-UI-Trigger und Aufräum-Logik unterstützt.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - Erbt von **ItemContainer** (SyncList-basierte Inventar-Logik, Mirror-Synchronisation)
        
    - Implementiert **IHandleInventoryComponent** (Defineslots-Interface)
        
    - Implementiert **ILootingEntity** (Methoden: `PopulateLoot`, `OnLooted`)
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **Server-Start**: `OnStartServer` führt Initialisierung durch, füllt Slots gemäß `_maxItems` leer oder via `PopulateLoot` initial gefüllten Slots.
        
    2. **Loot-Befüllung**: Aufruf von `PopulateLoot()` basierend auf Loot-Tabelle; verteilt Items auf Slots, prüft `_canStore` Typ-Whitelist.
        
    3. **Loot-Entnahme**: Client ruft `Cmd_LootItem(slotIndex)` → Server überprüft `CanRemove` und entfernt Item via `SERVER_Remove`.
        
    4. **Slot-Statistiken**: `SlotsFree`/`SlotsOccupied` liefern aktuelle Zustände.
        
- **Architektur-/Designentscheidungen**
    
    - **Interface-First**: `ILootingEntity` definiert Loot-Zyklus, erleichtert Erweiterung verschiedener Loot-Typen.
        
    - **Dynamische Slot-Anzahl**: Nutzt Serialized-Feld `_maxItems`, initialisiert dynamisch.
        
    - **Typ-Whitelist** (`_canStore`): Sichert, dass nur definierte ItemTemplates gelootet werden können.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - - **Wiederverwendbar**: Jede Entität mit Loot-Potenzial implementiert dieselbe Komponente.
            
    - - **Konfigurierbar**: Loot-Inhalte können per ScriptableObject-Tabellen definiert werden.
            
    - – **Komplexität**: Mehr Zustandslogik durch Loot-Zyklus, Tests notwendig.
        
    - – **Netzwerk-Overhead**: Zusätzliche Commands/SyncList-Events für Loot-Operationen.
        



## Tests

1. **Initialisierung & Loot-Befüllung**
    
    - Schreibe Loot-Tabelle mit bekannten Items, rufe `PopulateLoot` auf, prüfe Slots-Aufteilung und `_canStore`-Filter.
        
2. **Loot-Entnahme**
    
    - Simuliere Client-Loot: `Cmd_LootItem` mit gültigem Slot, prüfe `CanRemove` und Slot-Leerung.
        
    - Ungültiger SlotIndex oder leerer Slot → Ablehnung.
        
3. **ILootingEntity-Callbacks**
    
    - Verifiziere, dass `OnLooted()` nach erstem erfolgreichen Loot-Aufruf nur einmal pro Slot ausgelöst wird.
        
4. **Dynamische Slot-Anzahl**
    
    - Setze `_maxItems` auf verschiedene Werte, starte Server, prüfe Anzahl initialer Slots.
        
5. **Netzwerk-Synchronisation**
    
    - Zwei-Client-Test: Loot-Operation auf Client A aktualisiert Inventar auf Client B.
        

## Offene Punkte / Probleme

- **Race Conditions**: Gleichzeitiges Looten desselben Slots durch mehrere Clients muss synchronisiert werden.
    
- **Loot-Tabellen-Management**: Aktuell hardcodierte Loot-Tabelle-Referenz, könnte per Dashboard verwaltet werden.
    
- **Fehlerbehandlung**: Ungültige Slot-Zugriffe sollten spielerfreundliche Meldungen auslösen.