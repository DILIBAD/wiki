# TLDR

Zentrales Teilsystem zur Verarbeitung jeglicher Schadensereignisse: Validierung, Modifikation und Weiterleitung von Schaden über ein einheitliches Datenobjekt.

## Zweck / Ziel

Dieses Subsystem wurde entwickelt, um die reine Gesundheitsverwaltung von allen Damage-spezifischen Aspekten zu entkoppeln. Es löst folgende Probleme:

- Einheitliche Übergabe aller relevanten Schadensinformationen (Quelle, Ziel, Typ, Betrag)
    
- Zentrale Validierung (Fraktion, Lebenszustand, Invulnerabilität)
    
- Einfache Erweiterbarkeit (z. B. DoT, skalierter Schaden, Defense, Schwierigkeitsanpassung)
    
- Bereitstellung der Schadenquelle für nachgelagerte Systeme (z. B. Enmity, Statistik)
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `IDamageableEntity` / `ICombatEntity` (Quelle & Ziel)
        
    - `DamagePayload` als DTO für alle Schadensdaten
        
    - `ServiceLocator.FactionService` für Fraktionsprüfung
        
    - `ServiceLocator.GameLoopService` für Schwierigkeitsfaktor
        
    - Mirror-Netzwerk (SyncVar für Invulnerabilität, `[Server]`-Attribute)
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. Client oder Server initiiert `SERVER_ApplyDamage` auf dem Server
        
    2. Vorabprüfung via `CanReceiveDamage` (Fraktion, Lebenszustand, Invulnerabel)
        
    3. Anpassung des Schadenswerts (Defense, NPC-Skalierung)
        
    4. Übergabe an Health-Subsystem (`Health.SERVER_Modify`)
        
    5. Auslösen des Events `OnDamageReceived` für Beobachter
        
- **Architektur- oder Designentscheidungen**
    
    - DTO-Pattern (`DamagePayload`) für klare Verantwortungstrennung
        
    - Verwendung von Events statt direkter Rückrufe für lose Kopplung
        
    - Service Locator zur Laufzeitauflösung externer Abhängigkeiten
        
    - Lower bound auf Defense (0.1) um Division durch Null zu vermeiden
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - - Hohe Erweiterbarkeit (DoT, Statistik, Balancing)
            
    - - Wiederverwendbare Validierungslogik
            
    - – Mehraufwand durch zusätzliche Objekte und Netzwerktraffic
        
    - – Abhängigkeit von Service Locator erschwert Unit-Tests
        

## 

## Tests

1. **Fraktionsprüfung**
    
    - Schadenquelle und Ziel verschiedener Fraktionen → kein Schaden
        
    - gleiche Fraktion (oder erlaubte Konstellation) → Schaden angewendet
        
2. **Invulnerabilität**
    
    - Invulnerabel setzen → Schaden wird verworfen
        
    - Invulnerabel zurücksetzen → Schaden wirkt wieder
        
3. **Defense-Anwendung**
    
    - Variieren des Defense-Werts → resultierender Schaden skaliert korrekt
        
4. **NPC-Schadensskalierung**
    
    - Als NPC applizieren bei unterschiedlichen Difficulty-Stufen → Anpassung überprüfbar
        
5. **Event-Auslösung**
    
    - Listener an `OnDamageReceived` anbinden → erhaltene Nutzlast stimmt mit modifiziertem Schaden überein
        
6. **DoT-Erweiterung (manuell simuliert)**
    
    - Mehrfache Aufrufe auf dasselbe Ziel → erwartetes Verhalten (Summierung, Timer-Logik)
        

## Offene Punkte / Probleme

- Keine Batch-Verarbeitung von AoE-Schaden implementiert
    
- Caching von Payloads für Statistiken noch nicht realisiert
    
- Friendly-Fire-Ausnahmen müssten ergänzt werden
    
- Race-Conditions bei gleichzeitigen Schadensereignissen möglich