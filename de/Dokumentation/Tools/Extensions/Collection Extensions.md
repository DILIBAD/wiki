# Collection Extensions

## TLDR

Stellt häufig benötigte Methoden für Listen und Aufzählungen bereit: Duplikate finden, zufällige Elemente auswählen, Liste mischen und gewichtete Zufallsauswahl.

## Zweck / Ziel

Vermeidung redundanter Implementierungen derselben Collection-Operationen über mehrere Klassen hinweg.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `System.Collections.Generic.IList<T>` & `IEnumerable<T>`
        
    - `System.Linq` für Gruppierung und Selektion ListExtenstions
        
    - `UnityEngine.Random` für Zufalls­operationen RandomExtension
        
- **Ablauf und Interaktionen**
    
    - **FindDuplicates**: GroupBy → Count>1 → Schlüssel extrahieren
        
    - **Shuffle/Shuffled**: Fisher–Yates in-place oder auf Kopie
        
    - **GetRandom/PopRandom**: Index zufällig wählen, ggf. entfernen
        
    - **GetRandomElements**: Reservoir Sampling bzw. Teilmengen­entnahme
        
    - **WeightedRandomElement**: kumulatives Gewicht, Zufallswert, Schwellenvergleich
        
- **Architektur- oder Designentscheidungen**
    
    - Alle Methoden als generische Erweiterungen für maximale Wiederverwendbarkeit
        
    - Yield-basierte Implementierung für Deferred Execution bei Aufzählungen
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Performance vs. Lesbarkeit**: Linq erleichtert Code, kostet aber Allokationen
        
    - **Reproduzierbarkeit**: Abhängigkeit von UnityEngine.Random – keine Seed-Übergabe
        
    - **Fehlersicherheit**: Argumentprüfungen (null, leere Listen, negative Counts)
        

## Screenshots / Diagramme

- **Flussdiagramm**: Fisher–Yates Ablauf
    
- **Sequenzdiagramm**: Aufruf GetRandomElements → Sampling → Rückgabe
    

## Tests

1. **FindDuplicates**: Liste mit mehrfachen Schlüsselwerten, Ausgabe aller doppelten Schlüssel
    
2. **Shuffle**: Liste vor/nach Mischen vergleichen – gleiche Elemente, andere Reihenfolge
    
3. **GetRandomElement**: für Listen verschiedener Längen, Überprüfung auf zulässigen Index
    
4. **PopRandomElement**: Nach Entfernen Listenlänge und Inhalt prüfen
    
5. **GetRandomElements (n > Count)**: liefert ganze Liste in zufälliger Reihenfolge
    
6. **WeightedRandomElement**: Gewichte mit extremen Verhältnissen, statistische Verteilungstest
    
7. **Null-/Fehlerfälle**: Übergeben von null oder ungültigen Parametern → erwartete Exceptions
    

## Offene Punkte / Probleme

- **Keine Seed-Kontrolle** für deterministisches Testing
    
- **Speicher**: Kopien bei `Shuffled` und `GetRandomElements`
    
- **Thread-Sicherheit**: UnityEngine.Random nicht threadsafe