# NPC Animation Preview

# TLDR

Erlaubt Vorschau von Animationsclips aus einem AnimatorOverrideController eines NPC-EntityTemplates direkt im Editor-Window.

## Zweck / Ziel

Dieses Tool erleichtert das Testen und Visualisieren einzelner Animations-Clips (Skills) eines NPC-Templates ohne Play-Mode. Es löst das Problem, Animationsclips extern suchen und manuell previewen zu müssen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `NPCEntityTemplate` mit Referenz auf `AnimatorOverrideController`
        
    - `NPCAnimationPreviewWindow` (EditorWindow mit Preview-GUI)
        
- **Ablauf und Interaktionen**
    
    1. Window öffnen über **Tools → NPC Animation Preview**
        
    2. NPC_Template asset auswählen
        
    3. Animation ID per Dropdown wählen
        
    4. Override-Controller und Base Controller prüfen
        
    5. „Base Layer“ finden, „Skills“-State auslesen, Haupt-BlendTree und Sub-Trees durchsuchen
        
    6. Passenden Clip ermitteln („_S“-Suffix) und im Preview-Feld rendern
        
- **Architektur- oder Designentscheidungen**
    
    - Nutzung von `UnityEditor.Editor` für Clip-Preview
        
    - Fallback auf Original-Clip, wenn Override fehlt
        
    - Mehrstufige Validierung (OverrideController, RuntimeController, Layer, State, BlendTree)
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Fehlertoleranz:** Vielzahl an Zwischenprüfungen verhindert Null-Referenzen, führt aber zu komplexem UI-Flow
        
    - **Usability:** Dropdown-ID als Index, nicht als benannter Key, kann zu Verwirrung führen
        

## Screenshots / Diagramme

- **UI-Screenshot** mit Template-Selector, ID-Dropdown und Preview-Fenster
    
- **Sequenzdiagramm** für Schritte von Template-Auswahl über Clip-Suche bis Preview
    
- **Komponenten-Diagramm** der Beziehung zwischen OverrideController und Clip-Auswahl
    

## Tests

1. **Window-Aufruf:** Verfügbar und ohne Kritik beim Öffnen
    
2. **Template-Auswahl:** Ungültiges Template (ohne AnimatorOverrideController) zeigt Warnung
    
3. **ID-Dropdown:** Korrekte Anzahl an Einträgen entsprechend der Sub-BlendTrees
    
4. **Fehlerfälle:**
    
    - Kein Controller → Warnbox
        
    - Keine „Skills“-State → Info-Box
        
    - Kein Clip mit „_S“ → Info-Box
        
5. **Preview-Anzeige:** Für gültigen Clip interaktives Preview-GUI sichtbar
    

## Offene Punkte / Probleme

- Preview-Größe fest (256×256) nicht skalierbar
    
- Keine Möglichkeit, Animation in Schleife oder gezielt zu scruben
    
- ID-Mapping basiert rein auf Array-Index, nicht auf benannten States