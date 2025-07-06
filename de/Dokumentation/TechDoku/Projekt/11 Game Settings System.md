# TLDR

Zentrales Subsystem zum Laden, Anwenden, Persistieren und Bearbeiten aller Spiel-Einstellungen über Service und UI-Fenster.

## Zweck / Ziel

Dieses Teilsystem stellt eine einheitliche Infrastruktur bereit, um alle globalen Spiel-Einstellungen (Auflösung, Lautstärke, Grafikqualität, Hints, Sprache) konsistent zu verwalten. Es löst folgende Probleme:

- **Initialisierung & Laden:** Standardwerte beim ersten Start festlegen und gespeicherte Präferenzen laden
    
- **Anwenden:** Änderungen sofort auf Engine-APIs (Screen, Audio, Quality, Localization) übertragen
    
- **Persistenz:** Einstellungen zuverlässig in PlayerPrefs speichern
    
- **Benachrichtigung:** Andere Komponenten über Änderungen informieren (Event-Driven)
    
- **UI-Integration:** Einheitliches Settings-Fenster mit Save-/Cancel-Workflow
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **IGameSettingsService / GameSettingsService**: Kernlogik für Settings-Management
        
    - **ServiceLocator**: Beschaffung des Service und des LocalizationService
        
    - **PlayerPrefs**: Schlüssel-Wert-Speicherung
        
    - **Screen, AudioListener.volume, QualitySettings**: Laufzeit-Konfiguration
        
    - **LocalizationService**: Asynchrone Sprachumschaltung mit OnLanguageChanged
        
    - **SettingsWindow (TMP_Dropdown, Slider, Toggle, Button)**: Benutzeroberfläche
        
- **Ablauf & Interaktionen**
    
    1. **AfterStart (GameSettingsService)**
        
        - Prüft auf vorhandene PlayerPrefs-Keys, legt Standardwerte an
            
        - Filtert nur 16:9-Auflösungen, erstellt GameSettings-Instanz
            
        - Führt `ApplySettings()` aus und feuert `OnSettingsChanged`
            
        - Abonniert `LocalizationService.OnLanguageChanged` für nachträgliche Sprachupdates
            
    2. **ApplySettings**
        
        - Setzt Screen-Auflösung, AudioListener.volume, QualitySettings-Level
            
        - Löst `OnSettingsChanged` aus
            
        - Triggert `LocalizationService.SetLanguage(...)` (später `OnLanguageChanged`)
            
    3. **PreviewVolume**
        
        - Passt Lautstärke temporär an, ohne zu speichern
            
    4. **SaveSettings**
        
        - Schreibt aktuelle Werte in PlayerPrefs und ruft `PlayerPrefs.Save()` auf
            
    5. **SettingsWindow.Show()**
        
        - Klont aktuelle Settings für Abbruch
            
        - Füllt Dropdowns/Slider/Toggles mit Werten
            
    6. **UI-Interaktion**
        
        - **OnVolumeChanged:** ruft `PreviewVolume` auf
            
        - **OnSaveClicked:** liest UI-Werte, ruft `ApplySettings` + `SaveSettings`, schließt Fenster
            
        - **OnCancelClicked:** setzt Lautstärke zurück, verwirft Änderungen, schließt Fenster
            
- **Architektur- und Designentscheidungen**
    
    - **Event-Driven:** `OnSettingsChanged` für lose Kopplung
        
    - **Clone für Cancel:** Flaches Klonen der Original-Settings für Undo
        
    - **Trennung Vorschau vs. Persistenz:** Separate Methoden (`PreviewVolume` vs. `SaveSettings`)
        
    - **Direkte Service-Aufrufe aus UI:** Keine ViewModel-Schicht, einfacher aber weniger testbar
        
    - **Resolutions-Filter:** Einschränkung auf 16:9-Aspekte – bewusster Trade-off gegen breitere Unterstützung
        
- **Reflektion: Überlegungen & Trade-offs**
    
    - **PlayerPrefs:** Schnell & einfach, aber ohne Transaktionssicherheit
        
    - **Kein Fullscreen-Toggle:** Minimale Scope-Entscheidung
        
    - **Aspekt-Ratio-Beschränkung:** Verwirrend für Nutzer mit anderen Monitorformaten
        
    - **Race-Bedarf bei Sprachevents:** Einmaliges Abonnieren kann zu inkonsistentem Verhalten führen
        
    - **Inkonsistente Vorschau:** Nur Lautstärke hat Live-Feedback, andere Settings warten bis Save
        


## Tests

1. **Erster Launch**
    
    - PlayerPrefs löschen, Spiel starten → Standardwerte angelegt, Auflösung & Lautstärke gesetzt
        
2. **Persistenz-Laden**
    
    - Manuelle Änderung in PlayerPrefs, Neustart → geladene Werte stimmen mit Engine
        
3. **Auflösungs-Filter**
    
    - Mehrere Monitore anschließen → nur 16:9-Resolutions im Dropdown
        
4. **Lautstärke-Vorschau**
    
    - Slider bewegen → Volume ändert sich sofort, PlayerPrefs unbeeinträchtigt
        
5. **Save-Workflow**
    
    - Änderungen in UI → Save → Fenster schließt, neue Werte persistent, `OnSettingsChanged` gefeuert
        
6. **Cancel-Workflow**
    
    - Änderungen in UI (inkl. Lautstärke) → Cancel → Lautstärke zurückgesetzt, keine Persistenz
        
7. **Sprachwechsel-Event**
    
    - Sprache via LocalizationService ändern → `OnLanguageChanged` reagiert, UI-/Systemsprache aktualisiert
        

## Offene Punkte / Probleme

- **16:9-Only:** Keine Unterstützung anderer Seitenverhältnisse
    
- **Kein Fullscreen-Control** im SettingsWindow
    
- **Fehlende Eingabevalidierung** (Grafik-Index, Lautstärke-Grenzen)
    
- **Race Conditions bei Sprache-Events**: Mehrfache Abonnements möglich
    
- **Uneinheitliche UX:** Nur Lautstärke hat Live-Vorschau, andere Settings nicht
    
- **Schwache Testbarkeit:** Direkte Service-Aufrufe aus der UI ohne Abstraktionsschicht