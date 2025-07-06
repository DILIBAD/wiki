# Status Component Subsystem

# TLDR

Zentrales Netzwerk-fähiges System zur Verwaltung und Berechnung von Entitäten-Statistiken über ein Broker-Chain-Pattern mit dynamischen Modifikatoren (Items, Buffs, Upgrades).

## Zweck / Ziel

Das Subsystem wurde entwickelt, um die Verteilung und Anpassung von Basiswerten (z. B. Leben, Schaden, Bewegungsgeschwindigkeit) und darauf aufbauenden Effekten (Items, Buffs, Equipment, Upgrade-Level) in einem Mirror-basierten Multiplayer-Projekt einheitlich und performant zu gestalten. Es löst die Probleme unübersichtlicher, verstreuter Stat-Berechnungen und Synchronisationslogik, indem es:

- alle statischen Basiswerte serverseitig verwaltet und über SyncDictionaries mit Clients teilt,
    
- dynamische Anpassungen über ein standardisiertes Modifikator-Pattern (Broker-Chain) verarbeitet,
    
- automatisches Aufräumen abgelaufener Effekte ermöglicht,
    
- und Client-Server-Kommunikation für Echtzeit-Stat-Abfragen kapselt.
    

## Umsetzung

- **Relevante Kooperation:**
    
    - _StatusComponent_ als zentraler Einstiegspunkt für Setup, RPC-Abfragen und Mediator-Integration.
        
    - _StatsMediator_ als Broker für Query-Dispatch an alle aktiven Modifikatoren.
        
    - _StatModifier_ (abstrakt) und _TemplateStatModifier_ (konkrete Implementierung) für effektbezogene Logik mit Lebensdauer-Timer.
        
    - _Query_ repräsentiert eine einzelne Stat-Anfrage mit Basiswert und wird von allen Modifikatoren bearbeitet.
        
    - SyncDictionary-Strukturen für Basisstatistik und Upgrade-Level-Synchronisierung via Mirror.
        
- **Ablauf & Interaktionen:**
    
    1. **Initialisierung (Server):** Basiswerte werden gesetzt, Mediator instanziiert und Cleanup-Callback registriert.
        
    2. **Stat-Abfrage (Client):** Client sendet Command, Server führt Query im Mediator aus, Wert wird per TargetRpc zurückübermittelt und lokal gecached.
        
    3. **Modifier-Anwendung:** Beim Anwenden eines Buffs/Items wird ein neuer StatModifier erzeugt, beim Mediator registriert und sofort ausgewirkt.
        
    4. **Laufzeit-Handling:** Jeder Modifier tickt seinen internen Timer; bei Ablauf werden sie automatisch über das Dispose-Event entfernt (Mark-and-Sweep).
        
    5. **Upgrade-Integration:** Änderungen am SyncDictionary lösen Events aus, die Upgrade-Level-Changes weitergeben.
        
- **Architektur-/Designentscheidungen:**
    
    - Broker-Chain-Pattern trennt statische Basiswerte von dynamischen Änderungslogiken.
        
    - Mirror-SyncDictionaries für skalierbare Multiplayersynchronisation ohne Custom-Netcode.
        
    - Ereignisbasierte Automatik (OnDispose, OnStatsModified) entkoppelt Mediator, Component und visuelle Effekte.
        
    - Generische TemplateStatModifier-Klasse minimiert redundanten Code für Add-/Multiply-Operatoren.
        
- **Reflektion & Trade-offs:**
    
    - - Eingängiges Hinzufügen neuer Modifier-Typen ohne Mediator-Änderung
            
    - - Automatische Bereinigung reduziert manuellen Wartungsaufwand
            
    - – Event-Overhead bei vielen simultanen Modifikatoren
        
    - – Fehlende Batch-Verarbeitung könnte bei hohem Ticking-Volumen Performance-Engpässe verursachen
        
    - – Keine explizite Fehler- oder Last-Limitierung für SyncDictionary oder Modifier-Listen
        

## Screenshots / Diagramme

- **Sequenzdiagramm**: Stat-Abfrage vom Client bis zur Rückgabe des finalen Wertes.
    
- **Datenflussdiagramm**: Aufbau einer Query, Dispatch im Mediator, Verarbeitung in StatModifiern, finaler Wert.
    
- **UML-Klassendiagramm**: Beziehungen zwischen StatusComponent, StatsMediator, StatModifier, TemplateStatModifier und Query.
    
- **Event-Flow-Diagramm**: Lifecycle eines Modifiers (Anwendung, Timer-Tick, Disposal).
    

## Tests

1. **Initialisierungstest:**
    
    - Server startet eine Entity, ruft SERVER_Setup auf.
        
    - Verifizieren, dass alle Basiswerte im SyncDictionary liegen und keine Modifikatoren aktiv sind.
        
2. **Basis-Stat-Abfrage:**
    
    - Auf dem Client GetStat für einen Stat auslösen.
        
    - Sicherstellen, dass per TargetRpc der korrekte Basiswert zurückkommt und im lokalen Cache landet.
        
3. **Modifier-Applikation (Additiv & Multiplikativ):**
    
    - Buff mit Add-Operator anwenden, sofortige Erhöhung des Stats prüfen (z. B. Bewegungsgeschwindigkeit).
        
    - Buff mit Multiply-Operator anwenden, Multiplikation prüfen.
        
4. **Modifier-Ablauf:**
    
    - Modifier mit kurzer Dauer konfigurieren.
        
    - Laufzeit über mehrere Update-Zyklen simulieren, bis Ablauf.
        
    - Sicherstellen, dass nach Ablauf die Änderung nicht mehr wirksam und der Modifier vollständig entfernt ist.
        
5. **Item-Bindung & Entfernung:**
    
    - Mehrfaches Anwenden desselben Templates über verschiedene ItemInstances.
        
    - Entfernen einer ItemInstance und Verifikation, dass nur die zugehörigen Modifier entsorgt werden.
        
6. **Upgrade-Level-Sync:**
    
    - SERVER_SetUpgradeLevel aufrufen, Client-Seite prüft OnUpgradeLevelChanged-Event und aktualisierten Level.
        
7. **Konsistenz bei Netzwerkaussetzer:**
    
    - Simulierte Paketverluste: Mehrfaches RequestStat und Replay prüfen, dass Cache-Logik stabil bleibt.
        

## Offene Punkte / Probleme

- **Instanz-vs-Template-Mapping:** Unklare Handhabung, wenn dasselbe Template mehrfach global und instanzbezogen angewendet wird.
    
- **Concurrency:** Mögliche Rennzustände im Mediator beim parallelen Dispose- und Update-Aufrufen.
    
- **Skalierung:** Performance bei hunderten zeitgleich laufenden Modifiers und frequenten Stat-Abfragen.
    
- **Fehlerbehandlung:** Aktuell keine Absicherung gegen fehlgeschlagene RPCs oder ungültige Stat-IDs.
    
- **VisualEffect-Entfernung:** Auskommentierte SERVER_Remove-Aufrufe für Effektvorlagen müssen final implementiert werden.