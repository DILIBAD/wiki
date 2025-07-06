# TLDR

Zentrales Logging-System für Unity-Projekte, das Debug-Ausgaben gezielt nach Systemen und Log-Level verwaltet, leicht erweiterbar ist und zukünftig hierarchische Subsysteme unterstützen kann, um präziseres und granulareres Debugging sowie Monitoring zu ermöglichen.

## Zweck / Ziel

Das Logging-System wurde entwickelt, um Debug-Nachrichten projektweit übersichtlich und gezielt steuerbar zu gestalten. Es reduziert unübersichtliche und verstreute Debug-Statements und ermöglicht Entwicklern, Meldungen nach Subsystemen sowie Dringlichkeit (Log-Level) individuell zu filtern. Zukünftig sollen Unterkategorien (Subsysteme) hinzugefügt werden können, um die Granularität noch weiter zu erhöhen.

Langfristig könnte dieses System auch als Basis für ein Monitoring-System verwendet werden, um Ereignisse in Produktivumgebungen zu erfassen und auszuwerten.

## Umsetzung

### Relevante Kooperationspartner

- **Log.cs** (zentrale Verwaltung von Logs)
    
- **LoggerSettingsPreset.cs** (ScriptableObject zur Verwaltung verschiedener Logging-Presets)
    
- **LoggerConfiguratorWindow.cs** (Editor-UI zur komfortablen Verwaltung der Presets)
    

### Ablauf und Interaktionen

Das Logging-System unterscheidet verschiedene Log-Subsysteme (wie Core, Network, UI, AI, Inventory usw.), für die jeweils individuelle Einstellungen hinterlegt werden können (aktiviert/deaktiviert, Minimal-Level, Dev-Log-Anzeige). Die Einstellungen lassen sich zentral über den Unity-Editor konfigurieren und als Presets abspeichern.

Logs werden entsprechend der Einstellungen gefiltert und formatiert ausgegeben. Jedes Log enthält Informationen über das zugehörige System und den Log-Level sowie eine genaue Stacktrace-Information.

### Erweiterbarkeit und zukünftige Subsysteme

Die derzeitige Struktur ist bewusst modular und leicht erweiterbar gestaltet. Neue Systeme lassen sich einfach über die Enumeration `LogSystem` hinzufügen und erscheinen automatisch im Editor-Konfigurator. Das ermöglicht es Entwicklern, ohne großen Aufwand weitere spezifische Systeme zu ergänzen.

Eine zukünftige sinnvolle Erweiterung wäre die Implementierung einer hierarchischen Struktur für Systeme und Subsysteme (z.B. `AI → Pathfinding`, `Network → Authentication`). Dies würde es Entwicklern ermöglichen, Debug-Meldungen noch granularer zu filtern und so eine präzisere Kontrolle beim Debugging zu erreichen. Die dafür notwendige Anpassung sollte bereits frühzeitig bei der weiteren Entwicklung berücksichtigt werden, um späteren Refactoring-Aufwand zu minimieren.

### Architektur- oder Designentscheidungen

- **Leichte Erweiterbarkeit**: Neue Systeme können jederzeit unkompliziert hinzugefügt werden.
    
- **Potentielle hierarchische Struktur**: Bereits angedachte Erweiterung für noch granularere Log-Steuerung.
    
- **Zentrale Verwaltung**: Klar strukturierte zentrale Einstellungsverwaltung im Unity-Editor.
    
- **Voraussetzung für Monitoring-System**: Architektur ist darauf ausgelegt, zukünftige Integration eines Monitorings zu ermöglichen (z.B. via Grafana).
    

### Reflektion: wichtige Überlegungen und Trade-offs

Die Wahl einer flachen Struktur für Systeme wurde zunächst getroffen, um eine einfache Handhabung und schnelles Setup zu gewährleisten. Eine hierarchische Erweiterung hätte zwar höheren initialen Aufwand, würde aber den langfristigen Nutzen deutlich erhöhen, indem Debugging noch differenzierter ermöglicht wird.

Die Implementierung einer Subsystem-Logik sollte strategisch erfolgen, sobald das Projekt insgesamt ausgereifter ist und eine detailliertere Nachverfolgung verschiedener Unterbereiche notwendig wird.


## Tests

Manuelle Prüf- und Verifikationsschritte umfassen:

1. **Initialisierung und Konfiguration testen**
    
    - Editor öffnen und prüfen, ob alle vorhandenen Systeme inklusive neuer hinzugefügter Systeme angezeigt und konfigurierbar sind.
        
2. **Log-Ausgaben testen**
    
    - Logs in unterschiedlichen Systemen und Log-Levels erzeugen und prüfen, ob diese entsprechend der Einstellungen ausgegeben werden.
        
3. **Neue Systeme hinzufügen**
    
    - Erweiterung der `LogSystem`-Enumeration testen und prüfen, ob die neuen Systeme automatisch im Editor-Konfigurator erscheinen.
        
4. **Subsystem-Erweiterbarkeit simulieren**
    
    - Testweise Implementierung einer hierarchischen Struktur vorbereiten und konzeptionell validieren, wie eine granularere Steuerung möglich wäre.
        
5. **Persistenz-Tests**
    
    - Presets anlegen, speichern und neu laden, um sicherzustellen, dass alle Änderungen persistent bleiben.
        
6. **Runtime-Verhalten**
    
    - Überprüfung des korrekten Ladens von Presets und Verhalten in Runtime-Builds.
        

## Offene Punkte / Probleme

- **Subsystem-Erweiterung**: Aktuell noch nicht implementiert; technisches Konzept und mögliche Implikationen (Performance, Wartbarkeit) müssen noch evaluiert werden.
    
- **Projektweite Integration**: System bislang nicht vollständig integriert und genutzt; dies muss im nächsten Refactoring-Schritt erfolgen.
    
- **Monitoring-Einbindung (Grafana)**: Noch nicht realisiert; erfordert Proof-of-Concept.
    
- **Performancetests**: Bisher nicht explizit durchgeführt; Auswirkungen der Erweiterungen sollten vor Implementierung getestet werden.
    
- **Event-Logging und Analyse**: Konzept für zukünftige Speicherung und Auswertung von Events für Monitoring und Balancing steht noch aus.