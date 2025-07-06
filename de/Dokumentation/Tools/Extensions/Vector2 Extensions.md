# Vector2 Extensions

## TLDR

Ergänzt Unity-Vektoren um Konvertierungen, Drehung, Normalisierung, Abstände, Winkel und Interpolationen. Vector2Extensions

## Zweck / Ziel

Vermeidung von Boilerplate bei gängigen 2D-Vektoroperationen und Umwandlungen zu Vector3/Vector2Int.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `UnityEngine.Vector2`, `Vector3`, `Vector2Int`, `Mathf` Vector2Extensions
        
- **Ablauf und Interaktionen**
    
    - Methoden „ToVector3“, „ToVector2Int“ zur Umwandlung
        
    - „Rotate“, „Perpendicular“, „ClampMagnitude“, „SafeNormalized“ als reine Mathe-Operationen
        
    - „AngleTo“, „SignedAngleTo“, „DistanceTo“, „LerpTo“ für Winkel-/Entfernungs-Berechnungen
        
- **Architektur- oder Designentscheidungen**
    
    - Alle Methoden als Extensions für fließende Syntax
        
    - Intern größtenteils Delegation an Unity-APIs („Vector2.Lerp“, „Vector2.Angle“)
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Default-Toleranz in „IsNearlyEqual“ kann für manche Anwendungsfälle zu niedrig oder zu hoch sein
        
    - Performance-Kosten durch zusätzliche Methodebenen sind marginal
        

## Screenshots / Diagramme

- **Mathematisches Diagramm**: Vektorrotation und Normalisierung
    
- **Ablaufdiagramm**: Aufruf SafeNormalized → Toleranzprüfung → Rückgabe
    

## Tests

1. **Konvertierung**: Vector2 `(1.2, 3.4)` → Vector3 `(1.2,3.4,0)` bzw. `(1.2,3.4,5)`
    
2. **Rotation**: Vektor `(1,0)` um 90° → `(0,1)` prüfen
    
3. **Perpendicular**: Perp von `(x,y)` → `(-y,x)`
    
4. **ClampMagnitude**: Vektor mit Länge > maxLength auf Länge = maxLength reduzieren
    
5. **IsNearlyEqual**: zwei Vektoren innerhalb/nicht innerhalb Toleranz
    
6. **LerpTo**: t=0,0.5,1 → Start, Midpoint, End
    

## Offene Punkte / Probleme

- **Toleranzwert** in `IsNearlyEqual` evtl. konfigurierbar machen
    
- **Rundungsfehler** bei sehr großen Koordinaten