# Collider2D Extensions

## TLDR

Berechnet die exakte Fläche verschiedener 2D-Collider (Box, Kreis, Polygon, Kapsel, Composite) unter Berücksichtigung der Transform-Skalierung. Collider2DExtensions

## Zweck / Ziel

Ermöglicht spiel­logische Entscheidungen oder UI-Darstellungen basierend auf der realen Collider-Fläche statt nur des Bounding-Boxes.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `UnityEngine.Collider2D` und Subtypen (Box, Circle, Polygon, Capsule, Composite)
        
    - Geometrische Formeln: Rechteck, Kreis, Shoelace für Polygone Collider2DExtensions
        
    - `Transform.lossyScale` für weltweite Skalierung
        
- **Ablauf und Interaktionen**
    
    1. Pattern-Matching auf Collider-Typ
        
    2. Berechnung mit passender Formel
        
    3. Summation bei CompositeCollider2D über alle Paths
        
- **Architektur- oder Designentscheidungen**
    
    - Einfache statische Extension, kein zusätzlicher Komponenten-Overhead
        
    - Fallback auf Axis-aligned Bounding-Box bei unbekanntem Collider
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Shoelace-Algorithmus für Polygone kann Performance bei vielen Punkten kosten
        
    - Annahme konvexer Polygone; für stark konkave Shapes unter Umständen ungenau
        

## Screenshots / Diagramme

- **Geometrie-Diagramme**: Shoelace-Formel-Veranschaulichung und Zusammensetzung von Kapsel (Rechteck + Halbkreise)
    

## Tests

1. **BoxCollider2D**: bekannte Größe (z. B. 2×3) mit Scale 1,2 → Ergebnis = (2×1)×(3×2)=12
    
2. **CircleCollider2D**: Radius 1, Scale (2,3) → r = max(2,3)×1 = 3 → Fläche = π·3²
    
3. **PolygonCollider2D**: Rechteck-Shape → Shoelace vs. Handrechnung
    
4. **CapsuleCollider2D**: vertical/horizontal Mode, bekannte Größen
    
5. **CompositeCollider2D**: zwei Teilpfade addieren, Ergebnis summieren
    
6. **Fallback-Test**: Custom-Collider → Bounding-Box-Grundwert
    

## Offene Punkte / Probleme

- **Numerische Stabilität** bei sehr komplexen Polygon-Pfaden
    
- **Leistung** bei dynamisch häufig wechselnden Collider-Punkten
    
- **Unterscheidung** zwischen konvex/konkav nicht enthalten