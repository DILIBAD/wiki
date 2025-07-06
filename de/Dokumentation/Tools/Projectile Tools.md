# Projectile Tools

# TLDR

Editor-Werkzeuge zum massenhaften Bearbeiten und Validieren von ProjectileDataTemplates, inkl. NPC-spezifischer Filter und konfigurationsbasierter Grenzwertprüfung.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um Unity-Entwicklern und Game-Designern eine schnelle und zuverlässige Möglichkeit zu bieten, Parameter von Projektil-Vorlagen (ProjectileDataTemplate) zentral zu bearbeiten und automatisch gegen definierte Grenzwerte zu validieren. Dadurch wird die Konsistenz im Balancing gewahrt und Fehler im Asset-Workflow frühzeitig erkannt.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - _ProjectileDataTemplate_ (Datenvorlagen)
        
    - _StatsBatchConfig_ (Quelle für Difficulty-Namen)
        
    - _ProjectileValidatorConfig_ (ScriptableObject mit Min-/Max-Grenzwerten)
        
    - UnityEditor.AssetDatabase & SerializedObject zur Asset-Interaktion
        
- **Ablauf und Interaktionen (Unity & Editor)**
    
    1. Menüeinträge unter **Tools → Validator → Projectiles** öffnen die jeweiligen Fenster.
        
    2. Beim Start werden alle relevanten Templates aus dem Projekt geladen und nach Difficulty bzw. Fraktion gefiltert.
        
    3. _Batch Editor_ erlaubt manuelles Anpassen von Basisparametern (Damage, Speed etc.) und speichert alle Änderungen zentral.
        
    4. _NPC Batch Editor_ filtert zusätzlich nach Difficulty-Keyword und Fraktionspräfix, um nur NPC-relevante Templates anzuzeigen.
        
    5. _Validator Window_ lädt oder erzeugt ein Konfigurations-Asset und markiert abweichende Werte. Einzel- oder Sammelkorrektur („Fix“) richtet diese automatisch innerhalb der definierten Bereiche aus.
        
    6. Änderungen werden über `Save Assets` in der AssetDatabase persis­tiert und sind sofort im Inspector sichtbar.
        
- **Architektur- und Designentscheidungen**
    
    - **Trennung** von manueller Bearbeitung (Batch Editor) und automatischer Prüfung/Korrektur (Validator).
        
    - **Konfigurationsgetrieben**: Grenzwerte liegen in einem ScriptableObject, Änderungen erfordern keinen Code-Release.
        
    - **Namenskonventionen** als Filtermechanismus – erleichtern das Gruppieren, erfordern aber stringente Asset-Benennung.
        
- **Reflektion: Überlegungen & Trade-offs**
    
    - Vorteile: Schnelle Iteration, konsistente Datenqualität.
        
    - Einschränkungen: Starke Abhängigkeit von Namensschemata, fehlende Undo-Unterstützung für Massen-Fixes, keine komplexen Abhängigkeitsprüfungen zwischen Parametern.
        

## Screenshots / Diagramme

- **GUI-Abbildungen** der drei EditorWindows (Batch Editor, NPC Batch Editor, Validator).
    
- **Sequenzdiagramm**: Fenster-Start → Config-Laden → Asset-Laden → Validierung/Batch-Bearbeitung → Save.
    
- **Datenflussdiagramm**: Verbindung zwischen Config-Asset, AssetDatabase und SerializedObject.
    

## Tests

1. **Batch Editor öffnen** (Tools → Validator → Projectiles → Batch Editor).
    
2. **Parameter anpassen** (z.B. Damage verändern) → „Save All“ → Kontrolle im Inspector.
    
3. **NPC Batch Editor** öffnen → Tabs wechseln → prüfen, ob Templates korrekt nach Difficulty und Fraktion gefiltert werden.
    
4. **Templates hinzufügen/umbenennen** → „Refresh Templates“ → verifizieren, dass neue Assets erscheinen.
    
5. **Validator Window** öffnen → erstmaliges Starten erzeugt Config-Asset in Assets/Configs/Projectiles.
    
6. **Abweichende Werte testen**: Template außerhalb Grenzwerte bringen → „Refresh Templates“ → prüfen, dass Fehler-Markierung („✗“) erscheint.
    
7. **Einzelkorrektur („Fix“) und Sammelkorrektur („Fix All“) durchführen** → Kontrolle, dass Werte innerhalb des erlaubten Bereichs liegen.
    

## Offene Punkte / Probleme

- **Namensgebundene Filterung** kann unvollständig sein, wenn Assets nicht konsistent benannt.
    
- **Kein Undo** für automatisierte Korrekturen.
    
- **Erweiterbarkeit**: Hinzufügen neuer Parameter (z.B. Energieverbrauch) verlangt Code-Anpassungen in Validator und Config.
    
- **Kompatibilität** mit Mirror Networking und MST Master Server Toolkit muss weiter getestet werden, um Konflikte im Editor zu vermeiden.