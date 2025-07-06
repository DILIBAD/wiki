# TLDR

Ein zentrales System zur Verwaltung und Initialisierung von Services, um Modularität, Wartbarkeit und Flexibilität innerhalb des Unity-Projekts sicherzustellen.

## Zweck / Ziel

Dieses Teilsystem ermöglicht eine zentrale Verwaltung, Registrierung und Auflösung von Diensten (Services) sowie deren Lifecycle-Management (Start, Stopp, Neustart). Ziel ist die Vereinfachung der Kommunikation zwischen unterschiedlichen Systemen und Komponenten, wodurch die Modularität und Wartbarkeit der Anwendung verbessert wird.

Anstatt mehrere Singletons direkt anzusprechen, existiert ein zentraler ServiceLocator, der Zugriff auf alle Services bietet. Dies vereinfacht insbesondere die Entwicklung, da zentrale Funktionalitäten wie das Spawnen von Objekten oder NPCs in Services ausgelagert werden können. Änderungen an zentralen Mechanismen können somit an einer einzigen Stelle vorgenommen werden und wirken sich global aus.

## Umsetzung

### Relevante Kooperationspartner

- `ServiceBase`: Grundlegende abstrakte Implementierung von Services.
    
- `ServiceManager`: Verwaltung und Lifecycle-Management der Services.
    
- `ServiceLocator`: Zugriffspunkt für registrierte Services.
    
- `DefaultServiceFactory`: Standardisierte Initialisierung und Registrierung von Services.
    
- `GameBootstrap`: Einstiegspunkt zur Initialisierung von Services und Systemen.
    

### Ablauf und Interaktionen

- Beim Start der Anwendung (`GameBootstrap`) erfolgt die Initialisierung der Services durch Registrierung beim `ServiceManager`.
    
- Der `ServiceManager` verwaltet den Status der Services und löst ihre Abhängigkeiten auf. Services starten erst, wenn alle ihre Dependencies erfüllt sind.
    
- Über den `ServiceLocator` sind Services projektweit erreichbar. Initialisierungen werden automatisch über Unity-Callbacks gesteuert, sodass keine speziellen Szenen geöffnet sein müssen.
    
- Services implementieren die Interfaces `IService` bzw. `ISessionService` und können dadurch flexibel ausgetauscht werden.
    
- Erweiterte Interfaces für bestimmte Services, wie z.B. `IPlayerClassService`, bieten spezifische Eigenschaften und Methoden, die für besondere Anforderungen notwendig sind (z.B. Zugriff auf alle Klassen via `AllClasses`).
    

### Architektur- oder Designentscheidungen

- Nutzung eines zentralen `ServiceLocator`, um die Nachteile von Singletons zu umgehen und gleichzeitig eine lose Kopplung zwischen Komponenten zu gewährleisten.
    
- Die Service-Abhängigkeiten werden dynamisch aufgelöst und deren Lifecycle wird automatisch verwaltet.
    
- Implementierung von Interfaces sorgt für Austauschbarkeit der konkreten Services und vereinfacht Unit-Tests und Anpassungen.
    
- Spezialisierte Interfaces wie `IPlayerClassService` ermöglichen eine klar definierte und erweiterte Funktionalität, die speziellere Anforderungen erfüllt.
    

### Reflektion: wichtige Überlegungen und Trade-offs

- Die zentrale Verwaltung erleichtert Änderungen und Anpassungen, birgt jedoch das Risiko, dass bei einer fehlerhaften Implementierung einzelne Services schwerer zu debuggen sind.
    
- Dynamische Auflösung von Dependencies erhöht die Flexibilität, kann jedoch potenziell zu Laufzeitproblemen führen, wenn Abhängigkeiten nicht korrekt konfiguriert wurden.
    
- Ursprünglich war geplant, fehlende Dependencies dynamisch nachzuladen und Services bei Bedarf zu stoppen. Dies führte jedoch zu Endlosschleifen, weshalb temporär auf das dynamische Nachladen verzichtet wurde. Ein Refactoring sollte diesen Punkt erneut aufgreifen.
    
- Die Abstraktion durch Interfaces erhöht die Komplexität bei der initialen Einrichtung, verbessert langfristig jedoch die Wartbarkeit und Flexibilität.
    

## Screenshots / Diagramme

- **Diagramm 1**: Service-Lifecycle-Ablauf (WarmUp → Start → AfterStart → AfterSceneLoaded → Stop)
    
- **Diagramm 2**: Architektur-Übersicht mit `ServiceLocator`, `ServiceManager` und Services
    
- **Screenshot**: Beispielkonfiguration eines Services im Unity-Editor
    

## Tests

Folgende manuelle Prüf- und Verifikationsschritte sind erforderlich:

1. **Registrierung und Start der Services:**
    
    - Starten der Anwendung.
        
    - Prüfen, ob alle definierten Services korrekt im `ServiceManager` registriert und gestartet wurden.
        
2. **Auflösung von Dependencies:**
    
    - Überprüfen, dass ein Service erst startet, wenn alle seine Dependencies erfüllt sind.
        
    - Testen, ob bei fehlenden Dependencies entsprechende Fehlermeldungen angezeigt werden.
        
3. **Service-Zugriff:**
    
    - Prüfen, ob Services über den `ServiceLocator` abrufbar sind.
        
    - Verifizieren, dass austauschbare Implementierungen korrekt initialisiert werden.
        
4. **Lifecycle-Management:**
    
    - Prüfen, ob Services korrekt gestoppt und neu gestartet werden können.
        
    - Sicherstellen, dass die Reihenfolge der Lifecycle-Callbacks (WarmUp → Start → AfterStart → AfterSceneLoaded → Stop) eingehalten wird.
        

## Offene Punkte / Probleme

- Überprüfung und Behandlung von zirkulären Abhängigkeiten könnte verbessert werden.
    
- Fehlende detaillierte Dokumentation der spezifischen Service-Implementierungen.
    
- Aktuelle Implementierung erlaubt möglicherweise eine mehrfache Registrierung von Services, dies sollte genauer überprüft und ggf. verhindert werden.
    
- Es ist aktuell unklar, an welchen Stellen SessionServices bedenkenlos gestoppt werden können, da noch keine Probleme identifiziert wurden. Dies sollte weiter beobachtet und ggf. getestet werden.