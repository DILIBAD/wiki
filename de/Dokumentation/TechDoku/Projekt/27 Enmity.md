# Enmity System

# TLDR

Das Enmity System ermittelt für NPCs dynamisch das jeweils „bedrohlichste“ Ziel anhand von Distanz und verursacht­em Schaden. Es bietet eine erweiterbare, zero-GC-optimierte Lösung zur Priorisierung von Gegnern, ohne andere KI-Subsysteme anzupassen.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um NPCs eine intelligente Zielwahl zu ermöglichen. Es löst das Problem, dass NPCs bislang stur das nächstgelegene Objekt angegriffen haben, unabhängig davon, ob eine Einheit höheren Schaden verursacht. Durch das Enmity System weichen NPCs von statischen Angriffsmustern ab und reagieren adaptiv auf spielergesteuerte Eingriffe oder strukturelle Hindernisse.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **ICombatEntity / IDamageableEntity**: Schnittstellen für Schadensquelle und -empfänger.
        
    - **TickSystem**: Periodische Ausführung der Enmity-Logik in frei definierbaren Intervallen.
        
    - **Physics2D OverlapCircle**: Detektion von Einheiten im Sichtbereich.
        
    - **ObjectPool<EnmityEntry>**: Wiederverwendung der Einträge zur Vermeidung von Speicherallokationen.
        
    - **AnimationCurve (proximityCurve)**: Gewichtung des Entfernungsfaktors.
        
    - **InterfaceReference<ICombatEntity>**: Abstraktion des „Self“-Controllers.
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **OnStartServer**: Registrierung des Damage-Events und Initialisierung des TickSystems.
        
    2. **OnDamageReceived**: Erfassung von Zuträgern und Schadenswerten; Neuanlage bzw. Aktualisierung eines Enmity-Eintrags.
        
    3. **Tick** (Server-seitig):
        
        - **Detect**: Scan des Umfelds, Mindestbedrohung setzen, Neuanlage entfernungsbasierter Einträge.
            
        - **Decay**: Abschwächung bestehender Einträge über Zeit bzw. Entfernung. Automatisches Entfernen bei Unterschreitung.
            
        - **SortAndNotify**: Sortierung aller bekannten Einträge absteigend nach Bedrohungswert; Auslösen eines Events bei Zielwechsel.
            
- **Architektur- oder Designentscheidungen**
    
    - **Zero-GC-Philosophie**: Einsatz statischer Arrays und Objekt-Pools verhindert Laufzeitallokationen.
        
    - **Kapselung über Interfaces**: EnmitySystem bleibt entkoppelt von konkreten Entity-Implementierungen.
        
    - **Einsatz einer Distanzkurve**: Flexible Anpassung des Einflusses der Nähe ohne Codeänderung.
        
    - **Zirkulärer Puffer für DPS**: Konstante Speichergröße mit festem Zeitfenster für DPS-Berechnung.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Performance vs. Flexibilität**: Fest definierte Puffergrößen limitieren maximale Ereignisrate, verhindern aber Garbage Collection.
        
    - **Erweiterbarkeit**: Durch offene Hooks (EnmityEntry, proximityCurve, DPS-Fenster) leicht an neue Spielmechaniken anpassbar.
        
    - **Server-only Ausführung**: Konsistenz über Clients garantiert, erfordert aber eigenen Mechanismus, wenn Zielanzeige clientseitig nötig ist.
        



## Tests

1. **Reichweiten-Erkennung**
    
    - NPC platziert, zwei Entities in/außerhalb des Detection-Radius: nur intradistale Entity erzeugt einen Enmity-Eintrag.
        
2. **Schadens-Priorisierung**
    
    - Zwei Quellen richten unterschiedlichen Schaden an: Quelle mit höherem DPS- bzw. Gesamtschaden wird primäres Ziel.
        
3. **Nähen-Kurven-Gewichtung**
    
    - Identische DPS, aber unterschiedliche Distanzen: Nähe-Curve beeinflusst den Eintrag entsprechend.
        
4. **Decay und Entfernung**
    
    - Nach Verlassen des Radius reduziert sich die Bedrohung, bis der Eintrag automatisch verworfen wird.
        
5. **PrimaryTargetChanged-Event**
    
    - Schnell wechselnde Schadensquellen lösen korrekt das Event aus und NPC reagiert darauf.
        
6. **Pool-Robustheit**
    
    - Simuliere hohe Frequenz von Schaden- und Detect-Ereignissen: EnmityEntry-Pool gibt und entnimmt ohne Speicherlecks.
        

## Offene Punkte / Probleme

- **DPS-Curve** aktuell noch hart auf Null gesetzt, zukünftige Kurve fehlt.
    
- **MaxEvents & Fenstergröße** möglicherweise bei extrem schnellen Schlagabfolgen zu klein.
    
- **Fehlende Client-Visualisierung**: Primärziel wird nur serverseitig ausgewertet, Anzeige am Client erforderlich.
    
- **Fehlerbehandlung bei Pool-Erschöpfung**: Aktuell keine Fallback-Strategie definiert.