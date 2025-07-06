# NPC Behaviour

# TLDR

Das "NPC Behaviour"-Subsystem orchestriert das gesamte Verhalten von NPCs, einschließlich Phase-Management bei Gesundheitsänderungen, Fähigkeiten-Auswahl basierend auf Cooldowns und einer modularen State-Machine für AI-Entscheidungen.

## Zweck / Ziel

Dieses übergeordnete Teilsystem wurde entwickelt, um NPCs in einem Netzwerkspiel flexibel, datenorientiert und performant agieren zu lassen. Es löst folgende Kernanforderungen:

- Reaktive verhaltensbasierte Phasenwechsel bei Health-Updates
    
- Intelligente Wahl und Ausführung von Skills ohne manuelles Cooldown-Management
    
- Modularer Taktgeber und State-Machine für komplexe AI-Logik
    

Durch die Zusammenführung dieser drei Komponenten wird ein konsistentes und erweiterbares AI-Framework geschaffen, das Designer:innen und Entwickler:innen erlaubt, NPC-Verhalten ohne Code-Änderungen zu konfigurieren und zu testen.

## Umsetzung

### 1. Phase Management

- **Kooperationspartner**: `IDamageableEntity`, `INpcEntity`/`ICombatBuildingEntity`, `PhaseEffect`-ScriptableObjects.
    
- **Workflow**: Beim Serverstart Anmeldung auf NPC-Health-Events; jede Health-Änderung prüft vordefinierte Prozent-Schwellen; der niedrigste noch nicht ausgelöste Schwellenwert aktiviert das zugehörige `PhaseEffect`.
    
- **Designentscheidungen**: Datenorientierte Konfiguration über ScriptableObjects zur Entkopplung von Logik und Effektdefinition; Event-basierte Auslösung für unmittelbare Reaktion.
    

### 2. Skill Picker

- **Kooperationspartner**: `ICombatEntity`, `SkillsComponent`, `CombatComponent`, `ISkillDataTemplate`.
    
- **Workflow**:
    
    1. Prüfung mittels `HasUseableSkill`, ob mindestens ein Skill nicht auf Cooldown ist.
        
    2. Filterung off-CD-Skills → zufällige Auswahl (`GetRandomUseableSkill`) oder intelligente Auswahl mit Nutzen-Filter (`GetUseableSkill`).
        
- **Designentscheidungen**: Trennung der Auswahllogik von der Ausführung; LINQ-basierte deklarative Filterung; optionale Nutzen-Filterung für situative Intelligenz.
    

### 3. State Machine

- **Kooperationspartner**: `TickSystem`, `NpcStateMachine`, abstrakte `NpcState<T>`, konkrete State-Klassen (z. B. `IdleNpcState`, `WanderingNpcState`, `MovementNpcState`, `PickSkillState`, `AttackNpcState`, `EnrageState`, `InvulnState`).
    
- **Workflow**:
    
    1. `TickSystem` triggert periodisch `NpcStateMachine.Tick(entity)` bei lebendem NPC.
        
    2. Jeder State bewertet das Entity-Kontext (z. B. Boredom-Level, Vorhandensein von Zielen, Timer) über `Evaluate`; State mit höchster Priorität wird aktiviert.
        
    3. `Exit` des alten States → `Enter` des neuen States → `Execute` des aktuellen States.
        
- **Designentscheidungen**: Klares Separation of Concerns: Taktgeber vs. Zustandslogik; ScriptableObject-basierte States für Low-Code-Erweiterung und einfache Testbarkeit.
    
    - **Entkopplung**: Keine direkte Kopplung zwischen States; die States kennen sich nicht gegenseitig, was eine hohe Wartbarkeit und Austauschbarkeit ermöglicht.
        
    - **Kommunikation**: Nutzung des Blackboard Patterns über `ProvideContext` bzw. Memory zur losen Weitergabe von Zustandsdaten und Minimierung direkter Abhängigkeiten.
        

### 4. Building Behaviour

- **Analog zu NPC Behaviour**: Gebäude verhalten sich ähnlich wie NPCs im Phase Management und Skill Picker, verzichten jedoch auf einen `MovementNpcState`. Daher werden Movement-bezogene Details hier nicht im Detail beschrieben.
    
- **Refactoring-Hinweis**: Aktuell existieren separate StateMachine-Implementierungen für NPCs und Gebäude, was zu Code-Dopplungen führt. Als nächster Refaktorierungs-Schritt sollte hohe Priorität darauf gelegt werden, diese beiden Kontexte zu vereinheitlichen. Ziel ist es, gemeinsame StateMachine-Logik parametrisiert wiederzuverwenden und redundanten Code zu eliminieren, sodass keine separaten StateMachines mehr notwendig sind.
    

## Architektur-Reflektion

- **Datenorientierung vs. Performance**: ScriptableObjects bieten Flexibilität, benötigen jedoch Overhead beim Laden; Event- und Tick-basierte Mechanismen balancieren Reaktionsschnelligkeit und CPU-Last.
    
- **Modularität vs. Komplexität**: Einzelne, feinkörnige Komponenten (Phases, SkillPicker, States) erhöhen Wiederverwendbarkeit, erfordern aber klar definierte Interfaces und Context-Kontrakte.
    
- **Determinismus vs. Varianz**: Zufällige Skill-Auswahl fördert Varianz im Verhalten, erschwert jedoch reproduzierbares Balancing.
    

## Screenshots / Diagramme

- **Sequenzdiagramm**: HealthUpdate → PhaseManagement → PhaseEffect-Ausführung.
    
- **Ablaufdiagramm SkillPicker**: Cooldown-Prüfung → Filter → Skill-Auswahl → Trigger.
    
- **State Transition Graph**: Visualisierung aller `NpcState`-Implementierungen und ihrer Evaluierungsbedingungen.
    
- **Inspector-Screenshots**: Konfiguration von `PhaseEffect`, `SkillsComponent` und State-Definitionen als ScriptableObjects.
    

## Tests (Manuell)

1. **Phase Management**
    
    - NPC spawnen → Health-Events abonnieren?
        
    - Health-Reduktion auf definierte Prozentwerte → einmaliges Auslösen des korrekten Effekts.
        
    - Grenztests (genauer, knapp über/unter Schwellenwert).
        
    - Simultane Health-Änderungen mehrerer Quellen → keine Doppeltriggers.
        
2. **Skill Picker**
    
    - Verschiedene Cooldown-Konfigurationen → Rückgabe nur off-CD-Skills bzw. `null`.
        
    - Nutzen-Filter validieren (`IsUseful` true/false).
        
    - Häufigkeitsverteilung der zufälligen Auswahl über mehrere Durchläufe (Statistik).
        
3. **State Machine**
    
    - Test der Tick-Frequenz mit `IsAlive` true/false.
        
    - Szenarien für jede Context-Kombination (Movement vs. Combat vs. Idle vs. Enrage/Invuln).
        
    - Zustandsübergangsfolge Idle→Wandering→Movement→PickSkill→Attack→Enrage→Invuln, inklusive `Enter`/`Exit`-Aufrufe.
        
    - Timer-Grenzwerte (Boredom, Skill-Zwang, Invuln/Enrage).
        

## Offene Punkte / Probleme

- **Rollback** von Phasen bei Heilung nicht unterstützt.
    
- **Tie-Breaker** bei gleichhohen Prioritäten in StateMachine fehlt.
    
- **Netzwerk-Latenz** kann Tick-Synchronisation stören.
    
- **Blackboard-Cleanup** nach State-Abschluss unvollständig.