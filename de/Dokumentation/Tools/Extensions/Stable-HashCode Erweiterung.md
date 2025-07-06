# Stable-HashCode Erweiterung

## TLDR

Bietet eine plattformunabhängige, stabile Hash-Funktion für Strings, um auf allen Maschinen identische Werte zu erzeugen. HashCodeExtension

## Zweck / Ziel

Standard-`string.GetHashCode()` liefert je nach Laufzeitumgebung unterschiedliche Ergebnisse. Diese Erweiterung stellt sicher, dass Hash-Codes über Builds und Plattformen hinweg konstant bleiben, z. B. für Dictionary-Keys in verteilten Systemen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - System.String als Zieltyp
        
- **Ablauf und Interaktionen**
    
    1. Iteration über alle Zeichen im String
        
    2. Laufende Multiplikation/Akkumulation im `unchecked`-Kontext
        
    3. Rückgabe des resultierenden Int-Wertes
        
- **Architektur- oder Designentscheidungen**
    
    - Verwendung von Prime-basiertem Seed (23) und Multiplikator (31) für gute Verteilung
        
    - `unchecked` zur Kontrolle über Integer-Überläufe
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Weniger performant als native Implementierung, aber deterministisch
        
    - Kein Schutz gegen absichtliche Kollisions­attacken
        

## Screenshots / Diagramme

- **Ablaufdiagramm**: Zeichen → Schleife → Hash-Berechnung → Ergebnis
    

## Tests

1. **Konsistenz**: Auf mehreren Maschinen/Builds den gleichen String mehrfach hashen, stets identisches Ergebnis.
    
2. **Kollisionsprüfung**: unterschiedliche Strings vergleichen – Collisionsrate beobachten.
    
3. **Langstrings**: Performance und Ergebnisvalidität bei sehr großen Strings (> 10 000 Zeichen).
    

## Offene Punkte / Probleme

- **Kollisionsanfälligkeit** bei bestimmten Eingabemustern
    
- **Performance** gegenüber optimierten nativen Hash-Algorithmen