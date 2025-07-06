# 2D-Asset Generierung aus 3D-Assets (PoC)

# TLDR

Kurzer Prototyp, um aus vorhandenen 3D-Charakter- und Waffen-Assets per Render-, Rotations- und Pixel-Art-Konvertierung automatisiert 2D-Sprites zu erzeugen – mit Fokus auf Variation und Art-Style-Anpassung. Integriert ist nun die Unity-Komponente `DirectionalPixelateCaptureManager`, die Rotation entlang definierter Y-Winkel automatisiert, Diffuse- und Normal-Atlas erzeugt und direkt in Unity lauffähig ist.

## Zweck / Ziel

Wofür wurde dieses Teilsystem entwickelt? Welches Problem löst es?

- Reduzierung des manuellen Aufwands beim Erstellen konsistenter 2D-Sprites aus 3D-Modellen.
    
- Automatisierte Erzeugung von Richtungsvarianten (z. B. 8 Blickwinkel) und Normalmaps für realistische Schattierung.
    
- Schnelle Anpassung an verschiedenen Art-Styles dank Pixel-Art-Converter-Integration und Shader-Unterstützung.
    

## Umsetzung

### Relevante Kooperationspartner

- **3D-Charaktermodelle**: Meshes mit Rig/Bones.
    
- **Ausrüstungs-Meshes**: Waffen, Schilde u.Ä., im Rig verankert (Hand-Bone-Zuordnung).
    
- **DirectionalPixelateCaptureManager.cs** (Unity-Komponente) 
    
- **Blender**: Initiales Bone-Parenting (Hand-Bone ↔ Ausrüstungsobjekt).
    
- **Pixelate – Pixel Art Converter** (Unity Asset Store).
    
- **Custom Shader** (Hidden/ViewSpaceNormal) für Normalmap-Rendering.
    

### Ablauf und Interaktionen (Unity & Blender)

1. **Bone-Assignment in Blender**: Objekte an Hand-Bone parenten, um Offset-Fehler zu eliminieren.
    
2. **Konfiguration im Unity-Inspector**:
    
    - Ziel-GameObject (`_target`), Rotations-Transform (`_rotateTarget`), Quell-Animationen (`_sourceClips`), Sprite-Zellen-Größe (`_cellSize`), Kamerareferenz.
        
    - Y-Rotationen definieren (Standard: 0°, 45°, …, 315°).
        
3. **Editor- bzw. Runtime-Auslösung**: Aufruf von `CaptureAnimation` bzw. `CaptureFrame` im Skript.
    
4. **Render-Schleife**: Für jede Frame-Rotation-Kombination wird:
    
    - Animationspose mit `SampleAnimation` angewandt,
        
    - Diffuse-Atlas (RGBA) gerendert,
        
    - Normalmap über `RenderWithShader(Hidden/ViewSpaceNormal)` erstellt.
        
5. **Atlas-Erzeugung**: Diffuse- und Normal-Maps werden in großen Texture2D-Atlanten zusammengeführt.
    
6. **Pixel-Art-Konvertierung**: Atlas durch Pixelate-Prozessor schicken, FilterMode-Punkt anstelle Bilinear, Farbreduktion je Projekt-Shader.
    
7. **Import & Verwendung**:
    
    - Sprite-Sheets generieren, Pivots (`_pivot`) setzen, Material-Generierung optional (`_createAutoMaterial`).
        
    - Directe Einbindung in Animator-Controller und Shader-Graph für Pixel-Art-Look.
        

### Architektur- oder Designentscheidungen

- **Modularer Komponenteneinsatz**: Ein einzelnes MonoBehaviour für Capture, leicht erweiterbar.
    
- **External Tools vs. In-Editor**: Blender nur für initiales Rigging, alles weitere in Unity-Editor automatisierbar.
    
- **Daten-Pipeline**: Einheitliche Namenskonventionen (`{CharName}_{ClipName}_{Rot}_Diffuse.png`) und Ordnerstruktur.
    
- **Flexibilität**: Abschaltbare Features (`createNormalMap`, `pixelated`, `useAnimation`) für Test- und Produktionsläufe.
    

### Reflektion: wichtige Überlegungen und Trade-offs

- Bone-Parenting in Blender verhindert Latenz-Probleme, erfordert jedoch manuelle Vorarbeit pro Rig.
    
- Atlas-Layout (Grid vs. lineare Reihen) beeinflusst Memory und Texture2D-Grenzen (max. 4096×4096).
    
- Pixelate-Konvertierung beschleunigt Workflow, kann jedoch bei strengen Art-Guides feines Nachschärfen nötig machen.
    

## Screenshots / Diagramme

- **Blender-Rigging**: Hand-Bone-Passung in Blender.
    
- **Unity-Inspector**: Einstellungen des `DirectionalPixelateCaptureManager`.
    
- **Pipeline-Diagramm**: Von FBX-Export über CaptureManager zu Pixelate und SpriteSheet.
    

## Tests

1. **Bone-Assignment-Test**: In Blender prüfen, ob Ausrüstungsobjekte ohne Offset am Hand-Bone haften.
    
2. **Rotationstest**: `DirectionalPixelateCaptureManager` auf ein Dummy-Target anwenden, alle Y-Rotationen durchlaufen, visuell kontrollieren (Hand-Weapon-Follow).
    
3. **Atlas-Integrität**: Diffuse- und Normal-Maps prüfen, ob alle Zellen korrekt befüllt und transparent dargestellt sind.
    
4. **Pixelate-Settings**: Verschiedene `filterMode` und Farbreduktionsstufen testen, Kantenanalyse im Unity-Editor.
    
5. **Automatisierter Import**: Generierte SpriteSheets automatisch in Animator-Controller laden, Animationen abspielen.
    

## Offene Punkte / Probleme

- **Rig-Automatisierung**: Automatisches Bone-Parenting direkt in Unity wäre optimal, aktuell nur manuell via Blender.
    
- **Texture-Atlas-Größenlimit**: Große Kombinationen von Frames × Rotationen überschreiten ggf. 4096×4096-Limit.
    
- **Feinkalibrierung Shader**: Pixel-Art-Shader muss für verschiedene Umgebungen (Licht, Palette) angepasst werden.
    
- **Fehlende Runtime-Integration**: PoC bisher nur als Editor-Werkzeug, Erzeugung zur Laufzeit nicht implementiert.