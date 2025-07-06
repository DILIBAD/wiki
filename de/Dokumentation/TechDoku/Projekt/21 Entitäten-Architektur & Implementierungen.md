# Entitäten-Architektur & Implementierungen

# TLDR

Übersicht über die mehrschichtige Interface-Aufteilung im Entity-System und deren konkrete Klassen-Implementierungen; Reflexion über die Kopplung von NPCs und Buildings und Vorschläge für ein mögliches Refactoring.

## Zweck / Ziel

Sicherstellen, dass Entitäten über einfache Interface-Abfragen gefunden werden können und redundanter Code vermieden wird, indem gemeinsame Funktionalität zentral in übergeordneten Interfaces gekapselt wird.

## Umsetzung

- **Interface-Schichten**
    
    1. **Basis**
        
        - `IEntity` – Grundfunktionen (Netzwerk-ID, Transform, Lock/Sichtbarkeit, Get/Register)
            
        - `IDamageableEntity` – Health, DamageProcess, Effekte
            
    2. **Movement**
        
        - `IMovingEntity` – `IMovementComponent`
            
        - `IMovingCombatEntity` – kombiniert Kampf (`ICombatEntity`) mit Movement
            
    3. **Kampf**
        
        - `ICombatEntity` – CombatComponent, Equipment, Skills, Animation, CombatPosition, `SERVER_Setup(int id)`
            
    4. **Loot & Interaktion**
        
        - `ILootingEntity` – Interactions, Inventory
            
    5. **KI / Denken**
        
        - `IThinkEntity` – Threat, Kontext, Skill-Picker
            
        - `INpcEntity` & `ICombatBuildingEntity` – ergänzen StateMachine/ThinkComponent
            
    6. **Spieler-spezifisch**
        
        - `IPlayerEntity` – Input, Respawn, Quests, Crafting, Placement, Preview
            
- **Konkrete Klassen-Implementierungen**
    
    - **BaseEntity** – implementiert `IEntity`, Caching, Lifecycle, SyncVars (Visibility, Direction, Lock)
        
    - **CombatEntity** – erweitert `BaseEntity` um `ICombatEntity`-Funktionalität
        
    - **MovingCombatEntity** (über Interface) – wird in NPCs und PlayerEntities verwendet
        
    - **NpcEntity** – `INpcEntity`, Movement, Looting, KI-StateMachine, DropHandler, Respawn-Timer
        
    - **PlayerEntity** – `IPlayerEntity`, InputComponent, QuestTracker, Crafting, Preview, Deployable/BuildingPlacement, Respawn-Workflow
        
    - **BuildingEntity** – `IBuildingEntity`, Upgrades, Repair, Health-Setup, Visibility-Hooks
        
    - **CombatBuildingEntity** – `ICombatBuildingEntity`, kombiniert CombatEntity- und BuildingEntity-Logik
        

## Vermittlerrolle der Entitäten

- **Zentrale Anlaufstelle (Facade)**:
    
    - Jede Entity-Klasse bündelt Anfragen über generische `Get<T>()`-Aufrufe und leitet sie an spezialisierte Komponenten weiter.
        
    - Komponenten (Movement, Combat, Health, Stats, UI-Handler etc.) müssen nur über die Entity abgerufen werden, nicht direkt verkettete Abhängigkeiten.
        
- **Delegation & Koordination**:
    
    - **Lifecycle** (`OnStart`, `Update`, `Server_Start`, Hooks) verteilt Zustands- und Event-Aufrufe an alle registrierten Komponenten.
        
    - **Sync-Variablen** (Visibility, Direction, Lock) werden zentral geupdated und über Hooks auf Renderer, Animationen und AI-Komponenten verteilt.
        
- **Caching & IoC**:
    
    - `ComponentCache` in `BaseEntity` speichert einmal geladenes `Component`-Objekt für schnellen Zugriff und vermeidet wiederholte `GetComponent<T>()`-Aufrufe.
        
- **Mediator**:
    
    - Innerhalb einer Entity sorgt sie dafür, dass Komponenten nicht untereinander, sondern nur über die Entity kommunizieren (z.B. Health löst Death-Event, Entity fängt es und delegiert an SpawnService und UI).
        

## 
## Tests

1. **Interface-Lookup**
    
    - Aufruf von `Get<ICombatEntity>` auf verschiedenen Objekten, Prüfen der korrekten Typ-Ermittlung
        
2. **Doppelte Implementierungen**
    
    - Code-Inspektion, dass kein Combat-Setup zweimal für NPC und Gebäude dupliziert ist
        
3. **Kopplungsanalyse**
    
    - Verifikation, dass `BuildingEntity` und `NpcEntity` einzig durch Movement-Unterschied trennbar sind
        
4. **Mediator-Verhalten**
    
    - Gesundheitstod auslösen, prüfen, dass Entity die Death-Event-Delegation an SpawnService und UI korrekt vornimmt
        

## Offene Punkte / Refactoring

- **Kopplung NPC ↔ Buildings**
    
    - Nur Unterschied: Bewegungskomponente. Movement könnte in einer höheren Schicht (z. B. `IMoveable`) zentralisiert werden.
        
- **Mögliche Zusammenführung**
    
    - Überprüfung, ob `IMovingEntity` und `IBuildingEntity` zu einem gemeinsamen `IWorldEntity`/`IDynamicEntity` fusioniert werden können, um doppelten Code in StateMachine und Lifecycle zu vermeiden.
        
- **Pro & Contra Abwägung**
    
    - **Pro:** Weniger Interfaces, geringerer Duplikationsaufwand, klarere Lifecycle-Logik
        
    - **Contra:** Erhöhte Komplexität in Shared-Layer, mögliche Verletzung des Single-Responsibility-Prinzips
        
- Fundierte Entscheidung im Refactor erfordert Analyse der Auswirkungen auf andere Subsysteme (z. B. UI-Hooks, Statemachine-Trigger).