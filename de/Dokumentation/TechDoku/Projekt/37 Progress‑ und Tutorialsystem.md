# Progress‑ und Tutorialsystem

# TLDR

Verwaltet den Fortschritt von Aufgaben (Entries und Tasks) und steuert die Abfolge von Tutorial‑Stufen in einem einzigen, modularen Subsystem auf Basis eines Event‑Bus.

## Zweck / Ziel

Dieses Subsystem kombiniert zwei eng verwandte Funktionalitäten:

- **Progress Management:** Einheitliche Verwaltung von Fortschrittseinträgen (Entries) und zugehörigen Aufgaben (Tasks), die sowohl einmalige als auch zählbare Ziele abbilden.
    
- **Tutorial Orchestrierung:** Automatisierte Abfolge vordefinierter Tutorial‑Schritte, die auf den Fortschritten basieren und UI‑/Audio‑Feedback auslösen.
    

Ziel ist es, durch lose Kopplung über Event‑IDs und eine klare Trennung von Logik und UI eine wiederverwendbare Basis für Tutorials und zukünftige Quest‑Systeme zu schaffen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `IProgressEntry` & `IObjectiveTask` mit Implementierungen für Einzel‑ und Zählaufgaben
        
    - `GameEventService` (Event‑Bus) für die Ereignisverteilung
        
    - `TutorialService` (erweitert `ServiceBase`, implementiert `ITutorialService`/`ISessionService`)
        
    - `UIQuestTrackerWindow` & `AudioService` über `ServiceLocator` für Feedback
        
    - **Werkzeug**: `UniTask` für asynchrone Verzögerungen nach Abschluss einer Stufe
        
- **Ablauf und Interaktionen**
    
    1. **Initialisierung**: `TutorialService.WarmUp()` erzeugt eine Liste von `ProgressEntry`‑Instanzen mit Tasks und setzt den Index auf 0.
        
    2. **Aktivierung**: Bei `ClientInitialize()` abonniert der Service das Event‑Bus‑Konstrukt und aktiviert den ersten Eintrag über eine UI‑Methode.
        
    3. **Ereignisverarbeitung**: Jeder Report (`TriggerEvent`) wird an den aktuellen `ProgressEntry.ProcessEvent(eventId)` weitergeleitet, wobei Tasks sequentiell oder parallel geprüft werden.
        
    4. **Abschluss und Übergang**: Nach Erreichen aller Task‑Ziele feuert der Eintrag ein Completion‑Event, löst UI/Audio aus, wartet kurz (ca. 0,365 s) und aktiviert das nächste Element oder beendet das Tutorial.
        
- **Architektur- oder Designentscheidungen**
    
    - **Lose Kopplung** über stringbasierte Event‑IDs ermöglicht einfache Integration, erschwert jedoch Refactoring.
        
    - **Flag für Sequenzialität** in `ProgressEntry`, um zwischen paralleler und sequentieller Bearbeitung zu wechseln.
        
    - **Verzicht auf ScriptableObjects**: Schnelle Code‑basierte Definition, jedoch geringere Designer‑Freundlichkeit.
        
- **Reflektion: Überlegungen und Trade‑offs**
    
    - **Vorteile**: Hohe Flexibilität, Wiederverwendbarkeit für Quests, minimale Abhängigkeiten.
        
    - **Nachteile**: String‑IDs und leere Exception‑Blöcke erschweren Debugging; keine Kontext‑Payloads; fehlende Persistenz.
        


## Tests

1. **Initialisierungstest**:
    
    - `WarmUp()` und `ClientInitialize()`, Kontrolle, dass das erste Entry in der UI erscheint.
        
2. **Parallelitätsprüfung**:
    
    - Mehrere Tasks im Entry, Events in beliebiger Reihenfolge auslösen, Fortschrittsanteil und Abschluss validieren.
        
3. **Sequenzieller Modus**:
    
    - `_enforceSequential=true`, vorzeitiges Feuern eines späteren Events ignorieren, nur der nächste Task reagiert.
        
4. **Zählbare Tasks**:
    
    - `CountingObjectiveTask` mit Ziel >1, mehrfaches Event‑Firing und Zählerstand prüfen.
        
5. **Ablauf Tutorial**:
    
    - Gesamte Abfolge aller vordefinierten Einträge durchspielen, UI‑ und Audio‑Callbacks beobachten, zeitliche Verzögerung verifizieren.
        
6. **Stop/Server‑Modus**:
    
    - `Stop()` aufrufen, sicherstellen, dass keine weiteren Events verarbeitet werden; `ServerInitialize()` ohne UI‑Effekt aufrufen.
        

## Offene Punkte / Probleme

- **Debugging**: Nachverfolgung stringbasierter Event‑IDs ist auf großen Projekten umständlich.
    
- **Kontext‑Payloads**: Aktuell nur Event‑Keys; für komplexe Quest‑Logik wären zusätzliche Nutzdaten nötig.
    
- **Persistenz & Konfiguration**: Fehlende ScriptableObjects erschweren Designer‑Workflows und Laufzeit‑Anpassungen.