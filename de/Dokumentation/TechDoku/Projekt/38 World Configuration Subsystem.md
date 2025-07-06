# World Configuration Subsystem

# TLDR

Zentrale Verwaltung und Ausliefern aller Spielwelt-Konfigurationen: Asset Nexus (Assets, NPCs, Portale, Questgeber, Basen, Ressourcen, Wellen) sowie Fraktionsdaten (Namen, Beschreibungen, Karten, NPC-Spawn).

## Zweck / Ziel

Das World Configuration Subsystem fasst sämtliche ScriptableObject-basierten Konfigurationen der Spielwelt zusammen und stellt sie über einen einheitlichen Service und Interfaces zur Verfügung. Es löst die Fragmentierung verteilter Prefab- und Daten-Verweise, indem es:

- Die **Asset Nexus**-Konfiguration zentral lädt und Assets wie NPCs, Portale, Questgeber, Spieler- und Gegner-Basen, Ressourcen-Nodes und Wellen-Definitionen bereitstellt.
    
- Die **Fraktions-Konfiguration** (FactionConfigTemplate) verwaltet: Fraktionsname, Beschreibung, zugehörige Karten (inkl. Weltboss), NPC-Spawn-Einträge pro Fraktion.
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **AssetNexusService** (ServiceBase + MST/Mirror): Runtime-Service für Asset-Zugriff.
        
    - **AssetNexusConfigTemplate** + **ResourceNodeConfigs**: ScriptableObjectCached mit Referenzen zu allen Prefabs und Templates.
        
    - **WaveConfigTemplate**: Definiert Wave-Identifier und Spawn-Einträge.
        
    - **PlayerBaseConfigTemplate** / **IPlayerBaseConfigTemplate**: Spieler- und Gegner-Basen-Prefabs und Templates.
        
    - **ResourceConfigTemplate** / **IResourceConfigTemplate**: Identifier und Sprite für Ressourcen-Items.
        
    - **ServiceLocator.SpawnService**: Instanziiert Prefabs server- und clientseitig.
        
    - **FactionConfigTemplate** / **IFactionConfigTemplate**: Fraktionsdaten inklusive BossMap, Kartenliste, NPC-Einträge.
        
    - **FactionEntityConfigEntry** / **IFactionEntityConfigEntry**: Identifier + NPC-Template-Referenzen pro Fraktionseintrag.
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **WarmUp**: AssetNexusService liest erste AssetNexusConfigTemplate aus, befüllt interne Caches (z. B. Ressourcen-Dictionary).
        
    2. **Laufzeit-Getter**: Methoden wie `GetNpcPrefab()`, `GetPortal()`, `GetRandomQuestGiver()`, `GetPlayerBaseConfigTemplateByKey()` liefern direkt konfiguriertes Asset bzw. Template.
        
    3. **Ressourcen-Spawn**: `GetResourceNode(..)` verwendet SpawnService, um ein ResourceNode-Prefab zu instanziieren und übergibt Menge, Hits und visuelle IDs.
        
    4. **Fraktions-Abruf**: Gameplay-Systeme fordern über IFactionConfigTemplate Fraktionsname, Beschreibung, Karten und NPC-Einträge an, um Map-Lade- und Spawn-Logik zu steuern.
        
    5. **Fehler- und Logging-Strategie**: Fehlende Keys → KeyNotFoundException; keine Questgeber/Ressourcen → Warn-Logs.
        
- **Architektur- und Designentscheidungen**
    
    - **ScriptableObjectCached**: Automatische Caching-Registrierung aller Config-Assets ohne manuelles Drag & Drop.
        
    - **InterfaceReference**: Lose Kopplung von Daten- und Implementierungsschichten; einfache Austauschbarkeit im Editor.
        
    - **Single-Config-Ladeverfahren**: Nur erste gefundene AssetNexusConfigTemplate wird verwendet, um Mehrfach-Konfiguration zu vermeiden.
        
    - **Trennung Service ↔ Daten**: Service liefert nur aus, Datenhaltung in ScriptableObjects.
        
- **Reflektion: Überlegungen und Trade-offs**
    
    - _Pro_: Einheitliche Verwaltung, schnelle Laufzeit-Zugriffe, klare Trennung.
        
    - _Contra_: Nur eine Config aktiv, kein Lazy-Loading, fehlendes Pooling, Editor-Validierung unvollständig, keine Mehrseiten-Lokalisierung.
        

## Screenshots / Diagramme

- **Inspector-Screenshot** von AssetNexusConfigTemplate mit allen Referenzfeldern.
    
- **Inspector-Screenshot** von FactionConfigTemplate mit BossMap, Kartenliste und NPC-Entries.
    
- **Sequenzdiagramm**: WarmUp → Load Config → SetupResources → Laufzeit-Getter → SpawnService.
    
- **Klassendiagramm**: AssetNexusService ↔ ConfigTemplates ↔ Interfaces; FactionConfigTemplate ↔ FactionEntityConfigEntry.
    

## Tests

1. **WarmUp ohne Config**
    
    - Alle AssetNexusConfigTemplate-Assets entfernen → Spiel starten → Error-Log prüfen.
        
2. **WarmUp mit Config**
    
    - Mindestens eine Config anlegen → internes Ressourcen-Dictionary prüfen.
        
3. **Prefab-Getter**
    
    - `GetNpcPrefab()`, `GetPortal()`, `GetRandomQuestGiver()`, `GetQuestGiverByIndex()` mit gültigen/ungültigen Indizes testen → Rückgabe/Warnung.
        
4. **Key-Lookups**
    
    - `GetPlayerBaseConfigTemplateByKey()`, `GetEnemyBaseConfigTemplateByKey()` mit existierendem/nicht-existierendem Schlüssel → Rückgabe/Ausnahme.
        
5. **Wave-Logic**
    
    - `GetNpcIdentifierByWave()` mit vorhandenem/nicht-vorhandenem WaveKey → zufälliger Eintrag/Ausnahme.
        
6. **Ressourcen-Spawn**
    
    - `GetResourceNode("CrystalX", pos, amt, hits)` → SpawnService-Aufruf, Ressource im Spiel, Visual-Setup prüfen.
        
7. **Fraktions-Datenzugriff**
    
    - Alle FactionConfigTemplates per Debug abrufen → Name, Beschreibungen, Karten und NPC-Einträge validieren.
        
8. **Netzwerk-Integration**
    
    - Auf Server/Client prüfen, dass SpawnService-Aufrufe synchronisiert und korrekt initialisiert werden.
        

## Offene Punkte / Probleme

- **Einzel-Config-Ladeverfahren**: Mehrfach-Configs werden ignoriert.
    
- **Keine Editor-Validierung**: Duplikate, leere Listen und null-Referenzen nicht erkannt.
    
- **Fehlendes Pooling**: Ressourcenobjekte werden nicht wiederverwendet.
    
- **Unbenutzte Methoden**: `Start()`, `Stop()`, `ServerInitialize()`, `ClientInitialize()` sind leer.
    
- **Keine Lokalisierung**: Beschreibungen nicht multilingual.
    
- **Kein Versionsmanagement**: Änderungen an ScriptableObject-Struktur können Assets inkompatibel machen.
    
- **Performance-Risiken**: Keine Lazy-Loading-Strategie, mögliche GC-Spitzen bei vielen Instanziierungen.