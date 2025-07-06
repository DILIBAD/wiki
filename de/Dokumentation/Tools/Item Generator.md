# Item Generator

# TLDR

Generiert ScriptableObject-Assets für Items (Generic, Wearable, Useable) basierend auf flexiblen Namens-, Beschreibungs-, Stapelgrößen- und Stat-Vorlagen.

## Zweck / Ziel

Dieses Subsystem automatisiert die Erstellung von Item-Assets für verschiedene Themen-Kategorien (EasterEgg, Historical, Fantasy, SciFi, Steampunk, Mythology). Es löst mühsame manuelle Asset-Erstellung und sorgt für konsistente Namens- und Attribut-Generierung.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - `ItemGeneratorSettings` und zugehörige Settings-Klassen (NameSettings, DescriptionSettings, StackSizeSettings, StatSettings)
        
    - `ItemGeneratorWindow` (EditorWindow zur Steuerung der Generierung)
        
- **Ablauf und Interaktionen**
    
    1. Window öffnen über **Tools → Item Generator**
        
    2. Kategorie auswählen, Output-Ordner einstellen
        
    3. Counts für Generic, Wearable, Useable sowie Min/Max Stat-Modifier festlegen
        
    4. „Generate Items“ starten → für jede Kategorie Items nacheinander anlegen
        
    5. Für Wearable/Useable zufällige Modifikatoren aus StatSettings ziehen und als Separate Assets generieren
        
    6. Alle Assets im gewählten Ordner speichern und Projekt neu laden
        
- **Architektur- oder Designentscheidungen**
    
    - Trennung von Settings-Factory und Generierungs-Logik für hohe Wiederverwendbarkeit
        
    - Verwendung von `SerializedObject` für konsistente Asset-Initialisierung
        
    - Per Kategorie vorgefertigte Vorlagen via Factory-Klassen
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Zufälligkeit vs. Reproduzierbarkeit:** Derzeit keine Seed-Option, erschwert Debugging
        
    - **Ordnerstruktur:** Jeder Item-Name wird Ordnername, kann bei langen Namen zu Dateisystem-Limitierungen führen
        

## Screenshots / Diagramme

- **UI-Screenshot** des Item Generator Fensters mit Dropdowns und Eingabefeldern
    
- **Flussdiagramm** für die Generierungsschritte Generic → Wearable → Useable
    
- **Klassendiagramm** der Settings-Factory und ItemGeneratorSettings
    

## Tests

1. **Kategorie-Wechsel:** Settings laden sich passend zur Auswahl
    
2. **Ordner-Erstellung:** Output-Verzeichnis wird bei Bedarf angelegt
    
3. **Count-Eingaben:** Negative Werte abgefangen, Min/Max-Logik korrekt
    
4. **Generate Items:**
    
    - Generic: Nur Name und Description
        
    - Wearable/Useable: Anzahl Modifikatoren im richtigen Bereich, Assets korrekt referenziert
        
5. **Asset-Daten:** Nach Generierung prüfen, dass alle Felder befüllt und Assets ins Projekt importiert sind
    

## Offene Punkte / Probleme

- Fehlende Undo/Redo-Unterstützung
    
- Keine Konfiguration für Useable-Heal-Amount-Bereich
    
- Kein Fortschrittsbalken bei großer Item-Menge