# NPC Context Providing Subsystem

# TLDR

Aggregiert alle relevanten NPC-Kontexte zentral im Memory-Blackboard, sodass einzelne States keine eigenen Berechnungen durchführen müssen und Änderungen sofort überall wirksam werden.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um sämtliche per AI-Logik benötigten Kontextdaten eines NPCs (z. B. Positionen, Ziele, Zustandsflags) in einem typisierten Blackboard bereitzustellen. Dadurch entfällt die Duplizierung von Berechnungen in einzelnen States, Anpassungen an der Kontext-Aggregation erfolgen zentral und werden sofort in allen zustandsbasierten Komponenten reflektiert.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `Memory` (Key-Value-Store mit Default-Initialisierung)
        
    - `IProvideContextComponent` & `ITickableComponent` (Interfaces)
        
    - `BlackboardKey` (Enum für alle Schlüssel)
        
    - `INpcEntity` (Zugang zu Health, Enmity, CombatComponent)
        
    - `Mirror.NetworkBehaviour` (Server-Only Logik)
        
    - `NPCProvideContextComponentEditor` (Live-Anzeige im Inspector)
        
- **Ablauf und Interaktionen**
    
    1. **OnStartServer**: Abo auf EnmityComponent.PrimaryTargetChanged, um Zielwechsel zu erkennen.
        
    2. **Tick (Server-Modus)**: Jede Tick-Iteration sammelt aktuelle Rohdaten (Position, Ziel, Skill-Reichweite) und setzt abgeleitete Flags (`HasCombatTarget`, `CanAttack` etc.) ins Memory.
        
    3. **Expose für States**: States erhalten Kontexte via `ProvideContext(s)(IBrainContext)`, direkt aus diesem Memory.
        
- **Architektur- und Designentscheidungen**
    
    - _Typensicherheit_: Generische `Memory.Get<T>`-/`Set<T>`-Methoden verhindern Casting-Fehler.
        
    - _Separation of Concerns_: Kontext-Aggregation vollständig entkoppelt von Zustands-Logik; States lesen nur, sie schreiben nicht.
        
    - _Server-Only Execution_: Berechnung ausschließlich auf dem Host, Clients lediglich Lesezugriff.
        
- **Reflektion: Wichtige Überlegungen und Trade-offs**
    
    - _Performance vs. Wartbarkeit_: Zentrales Blackboard reduziert Redundanz, kann aber bei hoher Tick-Rate und vielen Keys Overhead verursachen.
        
    - _Direkter Memory-Zugriff an einigen Stellen_: Sollte im Refactoring in eigene Kontext-Properties überführt werden, um Lesbarkeit in States zu erhöhen.
        
    - _Kopplung von Memory-Einträgen und Kontexten_: Prüfung nötig, ob `BlackboardKey`-Werte stärker mit Kontext-Objekten verknüpft werden können, um Modali­tät und Wiederverwendbarkeit in anderen Projekten zu verbessern.
        


## Tests

1. **Default-Initialisierung**
    
    - GameObject mit `NPCProvideContextComponent` starten, sicherstellen, dass `BoredomLevel` und `LastKnownTargetPosition` im Inspector initial angezeigt werden.
        
2. **Raw Context Properties**
    
    - Manuell `TargetPosition`, `TargetTransform` und `TargetEntity` setzen, prüfen, ob Memory-Werte synchron angezeigt werden.
        
3. **Derived Flags**
    
    - Simuliere Ziel in Reichweite/außer Reichweite, überprüfe die Toggles für `HasMoveTarget`, `HasReachedTarget`, `CanAttack`.
        
4. **PrimaryTarget-Änderung**
    
    - In Play-Mode `EnmityComponent.PrimaryTarget` wechseln, validiere Update von `_lastKnownTargetPos`.
        
5. **Server-Only Execution**
    
    - Komponente im Client-Build ausführen, sicherstellen, dass `Tick` keine Veränderungen durchführt.
        

## Offene Punkte / Probleme

- **Refactoring direkter Memory-Zugriffe**: Stellenweise wird direkt `Memory.Get`/`Set` aufgerufen – prüfen, ob diese Zugriffe in eigene Context-Properties gebündelt werden sollen.
    
- **Thematische Gruppierung**: Kontext-Properties könnten in Sub-Klassen (z. B. „TargetContext“, „MovementContext“) zusammengefasst werden, um Übersichtlichkeit und Maintainability zu steigern.
    
- **Kopplung von Memory-Einträgen mit Kontexten**: Möglichkeit evaluieren, jeden `BlackboardKey` direkt an ein `IBrainContext`-Objekt zu binden, um Modali­tät und Wiederverwendbarkeit in anderen Projekten zu fördern.
    

---

