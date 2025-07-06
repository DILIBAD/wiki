# ScriptableObjectCached

# TLDR

Bietet eine lazy-initialisierte Caching-Schicht für alle ScriptableObject-Instanzen eines Typs, um wiederholte Ressourcen-Loads zu vermeiden und über ein stabil erzeugtes HashID-System zu identifizieren.

## Zweck / Ziel

Diese Klasse wurde entwickelt, um die Performance bei der Arbeit mit vielen ScriptableObject-Assets zu verbessern, indem sämtliche Instanzen eines Typs einmalig aus den Resources-Ordnern geladen und in einem statischen Dictionary abgelegt werden. Zugleich stellt sie eine konsistente, numerische Identifikation (HashID) bereit, um Objekte netzwerkübergreifend synchronisieren zu können.

## Umsetzung

- Relevante Kooperationspartner
    
    - UnityEngine.Resources: Laden aller Instanzen aus dem Ressourcen-Ordner.
        
    - System.Collections.Generic.Dictionary: Speicherung der Objekte im Cache.
        
    - Namenshashing-Erweiterung (GetStableHashCode): Erzeugung stabiler, eindeutiger Integer-IDs.
        
- Ablauf und Interaktionen (Unity & Networking)
    
    1. Beim ersten Zugriff auf die statische Eigenschaft `All` wird im Hauptthread ein einmaliges Resources.LoadAll<T> ausgeführt.
        
    2. Alle gefundenen Instanzen werden auf Namensduplikate geprüft; bei Konflikten wird ein Fehler geloggt.
        
    3. Die eindeutigen Items werden unter Verwendung ihres Namens-Hashcodes als Schlüssel in ein Dictionary eingetragen.
        
    4. Verbrauchende Systeme (z. B. das Netzwerkmodul von Mirror) rufen über `HashID` oder über `All` referenzierte Instanzen ab, ohne erneut Ressourcen zu laden.
        
- Architektur- oder Designentscheidungen
    
    - Lazy Loading: Verzögern des Ladens bis zum tatsächlichen Bedarf, um Initialisierungs-Overhead zu reduzieren.
        
    - Statischer Cache: Zentraler Speicher, um mehrfaches Laden zu vermeiden.
        
    - Hash-basiertes Mapping: Effiziente Schlüsselzuordnung statt string-basierter IDs im Netzwerk.
        
- Reflektion: wichtige Überlegungen und Trade-offs
    
    - Sicherheitsaspekt: Ressourcen-Assets müssen eindeutige Namen tragen, andernfalls treten Laufzeit-Errors auf.
        
    - Speicherverbrauch: Einmaliges Laden aller Assets kann bei vielen Objekten den Speicherbedarf erhöhen.
        
    - Threading-Limitierung: Ressourcen-Load nur im Main-Thread möglich, daher keine asynchrone Vorladung.
        

## Screenshots / Diagramme

- Flussdiagramm: Erstzugriff auf `All` → Ressourcen-Load → Duplikatprüfung → Cache-Befüllung.
    
- Klassendiagramm: ScriptableObjectCached mit statischem Dictionary und Instanz-Eigenschaft `HashID`.
    

## Tests

1. **Erst-Load validieren**
    
    - Szenario: Leerer Cache, mehrere SO-Assets vorhanden.
        
    - Erwartung: `All` enthält alle Instanzen, keine Fehlermeldungen im Log.
        
2. **Duplikat-Logging**
    
    - Szenario: Zwei Assets mit gleichem Namen.
        
    - Erwartung: Einrichtige Anzahl Einträge in `All`, Debug.LogError mit Hinweis auf doppelten Namen.
        
3. **HashID-Konsistenz**
    
    - Szenario: Mehrfaches Auslesen der `HashID` einer Instanz.
        
    - Erwartung: Immer gleicher Wert über verschiedene Aufrufe hinweg.
        
4. **Zugriff während Laufzeit**
    
    - Szenario: Netzwerkkomponente greift via HashID auf ein Objekt zu.
        
    - Erwartung: Gesuchte Instanz wird zuverlässig aus `All` zurückgegeben.
        
5. **Speicherverhalten**
    
    - Szenario: Profiler-Messung vor und nach Erst-Load großer Asset-Menge.
        
    - Erwartung: Einmalige Speicherzunahme, keine weiteren bei späteren Zugriffen.
        

## Offene Punkte / Probleme

- Der Cache entleert sich nicht automatisch, kein Unload-Szenario implementiert.
    
- Namenskonflikte führen nur zu Log-Fehlern, nicht zu Ausnahmen oder Fallbacks.
    
- Mögliche Erweiterung: Asynchrone Vorladung im Editor oder Build-Zeit-Validierung von Namen.