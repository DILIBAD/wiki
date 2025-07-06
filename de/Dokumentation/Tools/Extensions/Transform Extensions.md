# Transform Extensions

## TLDR

Bietet eine Methode, um alle direkten Kinder eines Transforms entweder sofort oder verzögert zu zerstören. TransformExtensions

## Zweck / Ziel

Schnelles Aufräumen von GameObject-Hierarchien in Runtime oder Editor, ohne in jeder Klasse eine eigene Schleife schreiben zu müssen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `UnityEngine.Transform`, `GameObject`, `Object.Destroy` / `DestroyImmediate` TransformExtensions
        
- **Ablauf und Interaktionen**
    
    1. Rückwärts­schleife von `childCount − 1` bis 0
        
    2. Wahl zwischen Instant- (Editor) und Runtime-Löschung
        
- **Architektur- oder Designentscheidungen**
    
    - Rückwärts­lauf um Indexverschiebungen beim Löschen zu vermeiden
        
    - Parameter `instant` für Editor-Skripting (`DestroyImmediate`) vs. Runtime (`Destroy`)
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Sofortiges Löschen im Editor erlaubt Undo nicht standardmäßig
        
    - Große Kindmengen können Frame-Spikes verursachen
        

## Screenshots / Diagramme

- **Sequenzdiagramm**: TransformExtensions → Schleifen-Iteration → Zerstörung
    

## Tests

1. **Runtime-Löschung**: Parent mit 5 Kindern, `instant=false`, Kinder verschwinden nach Frame
    
2. **Editor-Löschung**: `instant=true` im Editor-Modus, sofort weg
    
3. **Leere Hierarchie**: keine Exception, kein Effekt
    
4. **Nested-Children**: nur direkte Kinder, Enkel bleiben unberührt
    

## Offene Punkte / Probleme

- **Undo-Unterstützung** im Editor nicht integriert
    
- **Leistung**: Batch-Löschungen vs. Einzelschritte