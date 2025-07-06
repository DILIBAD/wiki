# Skill Tools

# TLDR

Editor-Verlängerungen zur Batch-Bearbeitung und Validierung von SkillDataTemplateBasisklassen (speziell Nahkampf-Angriffe) sowie zur Überprüfung von AudioKey-Zuordnungen.

## Zweck / Ziel

Bereitstellung eines konsistenten Workflows für die Konfiguration und Qualitätssicherung von Skill-Assets.

- _MeleeAttackBatchEditorWindow_ dient der manuellen Anpassung von Schlagparametern.
    
- _MeleeAttackSkillValidatorWindow_ stellt sicher, dass Range und Radius basierend auf AOE-Status und Konfigurationsfaktoren korrekt gesetzt sind.
    
- _SkillAudioKeyValidatorWindow_ überprüft, ob zugewiesene AudioKeys in den statischen Konstanten definiert sind.
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - _MeleeAttackSkillDataTemplate_ (Nahkampf-Skills)
        
    - _SkillDataTemplateBase_ (Basis für alle Skill-Varianten)
        
    - _MeleeAttackSkillValidatorConfig_ (Faktoren und Offsets für Range/Radius)
        
    - _AudioKeys_ (statische Klasse mit String-Feldern)
        
    - UnityEditor.AssetDatabase, SerializedObject, Reflection
        
- **Ablauf und Interaktionen**
    
    1. **Batch Editor**: Lädt alle Nahkampf-Skill-Vorlagen, sortiert nach Difficulty (StatsBatchConfig oder Default). Designer kann Schaden, Radius, Effektverzögerung und Reichweite manuell anpassen.
        
    2. **Melee Validator**: Erzeugt oder lädt Config-Asset in Assets/Configs/Skills. Berechnet erwartete Range (0 bei AOE, sonst SkillUseRange×Faktor) und Mindestradius (SkillUseRange + Offset bzw. Faktor×SkillUseRange + Offset). Markiert Abweichungen und bietet „Fix“/„Fix All“.
        
    3. **Audio Key Validator**: Nutzt Reflection, um alle statischen String-Felder aus AudioKeys und verschachtelten Typen zu sammeln. Templates werden nach Difficulty und Fraktion gruppiert. GUI zeigt Auswahlfeld für _audioKey und validiert gegen die gefundene Key-Liste.
        
- **Architektur- und Designentscheidungen**
    
    - **Modulare Configs** je Validator-Fenster für einfache Anpassung ohne Code-Release.
        
    - **Reflection** für dynamische Ermittlung aller AudioKeys, um Wartungsaufwand gering zu halten.
        
    - **Klar getrennte EditorWindows** für manuellen und automatischen Workflow.
        
- **Reflektion: Überlegungen & Trade-offs**
    
    - Automatische Korrektur sichert Konsistenz, kann aber individuelle Feineinstellungen überschreiben.
        
    - Reflection-Aufrufe können bei vielen Assets Performance-Einbußen verursachen.
        

## Screenshots / Diagramme

- **GUI-Screenshots**: Melee Batch Editor, Melee Validator, Audio Key Validator.
    
- **Sequenzdiagramm**: OnEnable → Config-Laden → Asset-Laden/Reflection → Draw per Template → Fix/Save.
    
- **Klassendiagramm**: MeleeAttackSkillDataTemplate ↔ MeleeAttackSkillValidatorConfig ↔ EditorWindow-Klassen.
    

## Tests

1. **Melee Batch Editor öffnen** (Tools → Validator → Skills → Batch Editor) → Parameter ändern → „Save All“ → Inspector-Check.
    
2. **Difficulty-Tabs** durchschalten → korrekte Filterung prüfen.
    
3. **Melee Validator** öffnen → Config erzeugen → Config-Werte (nonAoeRangeFactor, aoeOffset) im Inspector anpassen.
    
4. **Templates mit AffectedTargets=1 & >1** testen → „Refresh Templates“ → korrekte Erwartungswerte und Markierungen verify.
    
5. **Einzel- und Sammelkorrektur** durchführen → Werte nach Fix im Inspector kontrollieren.
    
6. **Audio Key Validator** öffnen → alle statischen Keys laden → ungültigen Key in Template setzen → Validierungsanzeige prüfen.
    
7. **Korrektur des AudioKeys** über PropertyField → „Key valid“ bestätigen.
    

## Offene Punkte / Probleme

- **Performance** bei großen Asset-Sammlungen durch Reflection und repetitives AssetLoading.
    
- **Erweiterung auf weitere Skill-Typen** (Fernkampf, Buffs) erfordert zusätzliche Validator-Logik und Config-Assets.
    
- **Dynamische AudioKeys** (zur Laufzeit generierte Keys) werden nicht erkannt.
    
- **Fehlende Undo-Funktionalität** für automatisierte Fix-Operationen.