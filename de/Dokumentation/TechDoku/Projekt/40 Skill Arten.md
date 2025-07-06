# SkillDataTemplateBase

# TLDR

Abstrakte Basis für alle Skills, vereint Metadaten (Cooldown, Animation, Lokalisierung) und steuert den kompletten Skill-Lebenszyklus auf Client und Server.

## Zweck / Ziel

Diese Klasse bietet eine zentrale Blaupause für sämtliche Skills im Projekt. Sie löst das Problem, wiederkehrende Eigenschaften und Abläufe (wie Cooldown, Animationen, Eingabeerfassung und Audiofeedback) nicht in jeder Skill-Klasse erneut implementieren zu müssen, sondern einmalig in einem wiederverwendbaren Template zu kapseln.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `ServiceLocator.LocalizationService` für Namen und Beschreibungen
        
    - `ServiceLocator.AudioService` für Soundeffekte
        
    - `ScriptableObjectCached<SkillDataTemplateBase>` für effizientes Caching
        
    - Interfaces: `ISkillDataTemplate`, `ILocalizable`
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    - Clientseitig verarbeitet `CLIENT_OnDown` Eingaben und löst optional `CLIENT_OnUp` aus
        
    - Über `Cmd_TryUseSkill` wird ein Server-Command zum Starten des Skills geschickt
        
    - Serverseitig folgen nacheinander:
        
        1. `SERVER_CastStarted`
            
        2. `SERVER_AnimationStarted`
            
        3. `SERVER_ExecuteEffect`
            
        4. `SERVER_AnimationCompleted`
            
        5. `SERVER_CastCompleted`
            
        6. `SERVER_CooldownStarted` & anschließend `SERVER_CooldownCompleted`
            
- **Architektur- oder Designentscheidungen**
    
    - Trennung von Client-/Server-Logik via virtuelle Methoden
        
    - Interface-Referenzen für maximale Flexibilität und Austauschbarkeit
        
    - Einheitliche Handhabung von Audio, Lokalisierung und Animationen
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **+** Verhindert Duplikation und stellt Konsistenz sicher
        
    - **–** Viele virtuelle Hooks können die Komplexität erhöhen
        
    - **–** Interface-Aufrufe können minimalen Performance-Overhead verursachen
        


## Tests

1. **Client-Eingabe**: Bei Tastendruck `CLIENT_OnDown` auslösen und Sound abspielen
    
2. **Server-Hooks**: Mit Debug-Ausgaben die korrekte Reihenfolge der Server-Methoden überprüfen
    
3. **Cooldown**: Cooldown-Werte setzen und sicherstellen, dass Skill während Abklingzeit nicht erneut gestartet werden kann
    
4. **Lokalisierung**: Namen und Beschreibung anhand der Keys validieren
    

## Offene Punkte / Probleme

- Fehlende Standard-Implementierung für Skill-Abbruch (z. B. Cancel durch Spieler)
    
- Keine Validierung auf ungültige oder fehlende Lokalisierungsschlüssel
    
- Potenzielle Race-Conditions bei parallelen Skillaufrufen
    

---

# AOE Skill Subsystem

# TLDR

Verwaltet Flächeneffekte mit optionalen Anzeige- und Verzögerungsmechanismen, inkl. Nahkampf-, Teleport- und Platzierungs-AoE.

## Zweck / Ziel

Ermöglicht es, verschiedene Arten von Flächenangriffen umzusetzen und dabei je nach Skilltyp Indikatoren, Verzögerungen oder Teleport-Effekte zu kombinieren.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `PhysicsOverlapScanner` für Zielerkennung im Umkreis
        
    - `DelayedAoeField` bzw. `AbstractAoeField` für AOE-Logik und Spawn
        
    - `ServiceLocator.SpawnService` für das Erzeugen von Indikator- und Aoe-Objekten
        
    - `Cysharp.Threading.Tasks` (UniTask) für asynchrone Verzögerungen
        
    - `IMovingCombatEntity` (bei Invisible-AoE)
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **Client** (bei Platzieren-AoE): Vorschau via `PreviewComponent` starten/stoppen
        
    2. **Server**:
        
        - Spawn eines Indikator-GameObjects
            
        - Verzögerung (`aoeDelay`) per UniTask
            
        - Animation abspielen, SkillLock setzen/entfernen
            
        - Schaden oder Teleport ausführen
            
- **Architektur- oder Designentscheidungen**
    
    - Verwendung von UniTask statt Coroutines für bessere Cancellation-Unterstützung
        
    - Gemeinsames AOE-Feld-Component (AbstractAoeField) für Wiederverwendbarkeit
        
    - Indikator und tatsächliches Feld getrennt, um Vorschau und Effekt zu entkoppeln
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **+** Asynchrone Steuerung erlaubt flexible Verzögerungen
        
    - **–** Komplexität durch CancellationToken und Exception-Handling
        
    - **–** Genauigkeit der Kollisionsradius-Erkennung muss manuell kalibriert werden („ToDo: Fix AOE Radius“)
        

## Tests

1. **Indikator-Anzeige**: Spawn-Position und -Größe beim Client prüfen
    
2. **Verzögerung**: Zeit bis zum tatsächlichen Effekt messen und mit konfiguriertem Wert vergleichen
    
3. **AOE-Schaden**: Betroffene Ziele im Umkreis ermitteln und Schaden applizieren
    
4. **Teleport-Skill**: Position vor und nach Teleport validieren, Health-Visibility toggelt korrekt
    
5. **Abbruch**: Bei CancellationToken soll Indicator zerstört und Locks aufgehoben werden
    

## Offene Punkte / Probleme

- Ungenaue AOE-Radius-Berechnung (ToDo in Code)
    
- Fehlendes Logging bei Fehlern im asynchronen Ablauf
    
- Performance bei vielen gleichzeitigen AOE-Feldern
    

---

# Projectile System

# TLDR

Stellt Konfigurationsdaten für Geschosse bereit und implementiert das serverseitige Spawnen sowie Flugverhalten.

## Zweck / Ziel

Trennung von Geschoss-Eigenschaften (Geschwindigkeit, Lebensdauer, Radius, Trefferzahl) und der eigentlichen Skill-Logik, um Mehrfachverwendung und Anpassbarkeit zu ermöglichen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `ProjectileDataTemplate` als Datencontainer
        
    - `IProjectile`-Interface für Laufzeit-Objekte
        
    - `ServiceLocator.SpawnService` für das Instanziieren von Geschossen
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. Skill löst `SERVER_ExecuteEffect` aus
        
    2. Berechnung von Spawn-Position (inkl. Höhencorrection) und Rotation basierend auf Blickrichtung
        
    3. Spawnen des Prefabs über `Server_Spawn`
        
    4. Initialisierung via `SetupProjectile` und Start des Fluges (`SERVER_Fly`)
        
- **Architektur- oder Designentscheidungen**
    
    - Interface-Referenz für Daten, um Lock-In der Skill-Klasse zu vermeiden
        
    - Höhe als separater Parameter, um Kollisionen am Boden zu reduzieren
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **+** Hohe Wiederverwendbarkeit von Daten- und Logik-Komponenten
        
    - **–** Hard-codierte Höhe kann in verschiedenen Umgebungen zu Problemen führen
        
    - **–** Fehlende Pooling-Strategie kann zu Performance-Einbrüchen führen
        

## Tests

1. **Spawn-Position**: Vergleichen der tatsächlichen Spawn-Koordinaten mit erwarteter Offset-Position
    
2. **Fluggeschwindigkeit & Lifetime**: Messen, ob Projektil sich korrekt bewegt und nach definierter Zeit zerstört wird
    
3. **Treffer-Logik**: MaxTargets und MaxPiercingCount korrekt einhalten
    
4. **Fehlerfall**: Warnung ausgeben, wenn Prefab kein `IProjectile` implementiert
    

## Offene Punkte / Probleme

- Keine Objekt-Pool-Verwaltung für häufige Spawns
    
- Höhe (_hightCorrection) fest im Template, schwer zur Laufzeit anzupassen
    
- Kein Feedback an Client bei fehlerhafter Projectile-Initialisierung
    

---

# Stat Modification Skills

# TLDR

Ermöglicht zielgerichtete Anpassung von Attribute-Komponenten, sowohl auf Gegner (Debuff) als auch auf sich selbst (Buff).

## Zweck / Ziel

Diese Teilsysteme dienen dazu, Statusänderungen (Erhöhung oder Reduktion von Stats) auf Entitäten anzuwenden, ohne dabei Damage- oder Heal-Logik zu mischen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `InterfaceReference<IModifyStatsDataTemplate>` für konfigurierbare Stat-Objekte
        
    - `ModifyStatsComponent` an ICombatEntity oder IDamageableEntity
        
    - `PhysicsOverlapScanner` (bei Debuff) für Zielerfassung
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    - **DebuffSkillDataTemplate**:
        
        1. Scan in einem Kreis vor dem Anwender
            
        2. Filter nach erhaltenem Schadensrecht (`CanReceiveDamage`)
            
        3. Bis zu `AffectedTargets` Ziele auswählen und Stat-Modifikationen anwenden
            
    - **TargetSelfDataTemplate**:
        
        1. Iteration über alle referenzierten Stat-Templates
            
        2. Anwendung auf den Anwender selbst
            
- **Architektur- oder Designentscheidungen**
    
    - Trennung in Self- und Target-Komponenten für klare Verantwortungsbereiche
        
    - AffectedTargets-Limitierung zur Steuerung von Skalierung und Performance
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **+** Hohe Flexibilität durch Interface-Referenzen
        
    - **–** Keine automatische Rücknahme zeitlich begrenzter Buffs
        
    - **–** Fehlende Validierung auf korrekte Reihenfolge bei mehrfachen Modifikationen
        


## Tests

1. **Self-Buff**: Stat-Erhöhung prüfen und sicherstellen, dass nur Anwender betroffen ist
    
2. **Debuff**: Nur schädigbare Gegner im Radius anwenden, Filter auf `CanReceiveDamage` validieren
    
3. **AffectedTargets**: Maximalzahl der modifizierten Entitäten begrenzen
    
4. **Mehrfachanwendung**: Stapelbarkeit und Reihenfolge der Stat-Modifikationen prüfen
    

## Offene Punkte / Probleme

- Keine zeitliche Begrenzung oder automatische Reversion von Statänderungen
    
- Fehlende Synchronisation, falls mehrere Debuffs gleichzeitig wirken
    
- AffectedTargets darf nicht größer als verfügbares Ziel-Pool sein
    

---

# HealSkillDataTemplate

# TLDR

Heilt verbündete Ziele im Umkreis, optional mit visuellem Effekt über ein Prefab.

## Zweck / Ziel

Implementiert Heilzauber, die verbündete Entitäten basierend auf Fraktionszugehörigkeit regenerieren und dabei ein optisches Feedback erzeugen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `PhysicsOverlapScanner` für Bereichsscan
        
    - `ServiceLocator.FactionService` für Freund-Feind-Filter
        
    - `ServiceLocator.SpawnService` zum Erzeugen von Heal-Prefabs
        
    - `IVisualEntityEffectTemplate` für visuelles Heileffekt-Overlay
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. Scan in einem Kreis vor dem Anwender mit Layer-Maske
        
    2. Filter auf lebende Entitäten und Fraktionskompatibilität
        
    3. Spawn von Heal-Prefab bei konfigurierter HealAmount-Unterschiedlichkeit
        
    4. Serverseitige Änderung der Health-Werte (`SERVER_Modify`)
        
    5. Anwendung visueller Effekte via `SERVER_Apply`
        
- **Architektur- oder Designentscheidungen**
    
    - Trennung von Heal-Logik und visuellem Feedback
        
    - Verwendung von Fraktionsservice zur Verhinderung von „Heal-Farming“ bei Gegnern
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **+** Klare Trennung von Gameplay- und VFX-Logik
        
    - **–** Kein Overheal-Limit im Code (abhängig vom Health-System)
        
    - **–** HealAmount = 0 führt trotzdem zu Spawns, wenn Prefab gesetzt ist
        



## Tests

1. **Fraktionsfilter**: Nur befreundete Entitäten im Radius heilen
    
2. **HealAmount**: Prüfung von positiven, negativen und Null-Werten
    
3. **AffectedTargets**: Begrenzung auf definierte Anzahl von Heilenden
    
4. **VFX**: Sichtbarkeit und Position des Heal-Prefabs kontrollieren
    

## Offene Punkte / Probleme

- Fehlende Überprüfung auf maximale Gesundheit (Overheal)
    
- Heal-Prefab wird auch bei HealAmount = 0 gespawnt
    
- LayerMask-Konfiguration kann ungewollte Ziele einschließen
    

---

# PlaceTurretSkillDataTemplate

# TLDR

Erlaubt Spielern, stationäre Geschütztürme innerhalb einer Reichweite zu platzieren, inkl. Voransicht und Server-Spawn.

## Zweck / Ziel

Dieses System unterstützt das Platzieren von Deployables (z. B. Türme) durch Spieler innerhalb einer definierten Distanz und stellt eine visuelle Platzierungsvorschau bereit.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `PreviewComponent` für Platzierungsvorschau
        
    - `CombatBuildingTemplate` als Datenträger für Turret-Prefab
        
    - `DeployablePlacement` über `Server_SpawnDeployable`
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **CLIENT_OnDown**: Aufruf von `StartPreview` mit Preview-Prefab und Range
        
    2. **CLIENT_OnUp**: Stoppen der Vorschau, anschließender `Cmd_TryUseSkill`-Aufruf
        
    3. **SERVER_ExecuteEffect**: Validierung des Templates, `Server_SpawnDeployable` für den Turret
        
- **Architektur- oder Designentscheidungen**
    
    - Trennung von Vorschau und finalem Spawn über verschiedene Methoden
        
    - Nutzung der Mirror-Command-Logik (`Cmd_TryUseSkill`, `NetworkServer.Spawn`)
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **+** Klare Nutzerführung durch Preview
        
    - **–** Kein Schutz vor Überschreitung von MaxTurretCount auf Serverseite
        
    - **–** Vorschau wird nicht automatisch abgebrochen, wenn Skill auf Cooldown geht
        


## Tests

1. **Reichweitenprüfung**: Vorschau darf nur innerhalb von `Range` angezeigt werden
    
2. **Turret-Limit**: MaxTurretCount serverseitig erzwingen und validieren
    
3. **Preview-Cancel**: Unterbrechen der Vorschau bei Abbruch oder Cooldown
    
4. **Spawn-Position**: Turret-Objekt erscheint exakt an gewünschter Stelle
    

## Offene Punkte / Probleme

- MaxTurretCount wird nicht serverseitig durchgesetzt
    
- Fehlende Fehlermeldung bei nicht zugewiesenem Turret-Prefab
    
- Vorschau und effektive Platzierung teilen sich nicht dieselbe Range-Logik