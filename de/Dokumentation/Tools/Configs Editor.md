# Configs Editor

# TLDR

Zentrales Editor-Tool zur Verwaltung von Faction-, Entity- und State-Config-Assets über Sidebar-Tabs und Detail-Inspektor.

## Zweck / Ziel

Dieses Window dient dazu, alle relevanten Konfigurations-Assets (Fraktionen, NPC-Entitäten, Zustände) an einer Stelle anzuzeigen, auszuwählen und zu bearbeiten. Es löst das Problem verteilter Asset-Inspektoren und sorgt für konsistente Konfiguration im Projekt.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `FactionConfigTemplate`, `NPCEntityTemplate`, generische `ScriptableObject` für States
        
    - `ConfigsEditorWindow` als EditorWindow
        
- **Ablauf und Interaktionen**
    
    1. Window öffnen über **Window → Configs Editor**
        
    2. Beim Fokussieren alle Assets aus vordefinierten Resource-Ordnern laden
        
    3. Sidebar mit „Factions“, „Entities“, „States“-Tabs
        
    4. Liste der Assets pro Tab anzeigen; per Klick Detailseite laden
        
    5. ReorderableList für Faction-Entities; Default-Inspector für Entities/States
        
    6. Änderungen speichern über „Save Faction“ bzw. automatische AssetDatabase-Aktualisierung
        
- **Architektur- oder Designentscheidungen**
    
    - Einsatz von Enum-basierten Tabs für klare Trennung
        
    - Dynamisches Nachladen beim Fokus-Wechsel, um externe Änderungen zu reflektieren
        
    - Wiederverwendung des Standard-Inspektors für generische Objekte, um Code-Redundanz zu vermeiden
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Reload-Frequenz vs. Responsivität:** Assets bei jedem Fokus neu laden, kann UI-Hänger verursachen
        
    - **Erweiterbarkeit:** Aktuell auf drei Typen beschränkt; neue Typen müssten manuell ergänzt werden
        

## Screenshots / Diagramme

- **UI-Screenshot** des Fensters mit Sidebar und Detail-Panel
    
- **Komponenten-Diagramm** zur Veranschaulichung der Pfad-Konstanten und Asset-Caches
    
- **Sequenzdiagramm** für „OnFocus → LoadAll → InitFactionEntityList → Repaint“
    

## Tests

1. **Window-Aufruf:** Über das Menü erreichbar
    
2. **Asset-Laden:** Alle Templates aus den definierten Ordnern sichtbar
    
3. **Tab-Wechsel:** Inhalte aktualisieren sich korrekt
    
4. **Detail-Ansicht:** Faction-Properties im Inspector editierbar
    
5. **ReorderableList:** Hinzufügen/Entfernen von Entities in FactionConfigTemplate funktioniert
    
6. **Save Faction:** Änderungen bleiben nach Editor-Neustart erhalten
    

## Offene Punkte / Probleme

- Keine Undo-Unterstützung für Faction-Entitäten-List
    
- Fehlende Fehlerbehandlung bei ungültigen Ordnerpfaden
    
- Stati-Tab aktuell nur lesend, keine Editier-Möglichkeit