# Dialog-Interaktionssystem

# TLDR

Ermöglicht Spielern, im Netzwerk mit NPCs zu interagieren und steuert dabei die Auswahl, Anzeige und Fortsetzung von Dialogen (Kombination aus DialogInteractable, DialogFactory und den Dialog-/DialogEntry-Datenstrukturen).

## Zweck / Ziel

Diese Komponente bündelt die Client-seitige Interaktionslogik und das Dialog-Management für NPCs in einem Mirror- und MST-basierten Unity-Projekt. Sie löst folgende Probleme:

- Konsistente Erfassung und Steuerung von Spieler–NPC-Interaktionen
    
- Dynamische Auswahl und Verwaltung mehrsprachiger Dialoginhalte
    
- Zustandsbewusste Darstellung und Fortsetzung laufender Unterhaltungen
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - _LocalizationService_ für sprachabhängige Interaktionstexte
        
    - _UIService & DialogWindow_ für Anzeige und Steuerung des Dialogfensters
        
    - _DialogFactory_ zur zentralen Registrierung und Zufallsauswahl von Dialoglisten
        
    - _Dialog & DialogEntry_ als Datenmodelle für mehrsprachige Einträge
        
    - _ILootingEntity / IPlayerEntity_ zur Identifikation des interagierenden Spielers
        
- **Ablauf und Interaktionen**
    
    1. Spieler löst Interaktion aus → `CLIENT_Interact` ruft `CLIENT_OpenUI` auf
        
    2. Prüfung: Ist ein Dialog aktiv?
        
        - Ja → `ContinueDialog()` wird aufgerufen
            
        - Nein → Ausgewählten Dialog laden, anzeigen und bei Abschluss Flag zurücksetzen
            
    3. Nach Beendigung oder Abbruch wird die Interaktion über das Player-Interaction-System beendet
        
- **Architektur- und Designentscheidungen**
    
    - **Client-zentriert**: UI-Logik und Zustand nur auf Client, um Latenz zu minimieren
        
    - **Statische Initialisierung**: Alle Dialoge werden beim Programmstart in DialogFactory registriert
        
    - **Einfaches Zustands-Flag**: Minimale Komplexität für Fortsetzungslogik, aber potenzielle Race-Conditions
        
- **Reflektion und Trade-offs**
    
    - _Vorteile_: Schnellere UI-Reaktionen, einfacher Wartungsaufwand für Dialogdaten
        
    - _Nachteile_: Kein serverseitiges Backup oder Validierung des Dialog-Zustands, hoher Speicherbedarf beim großen Dialogumfang
        

## Screenshots / Diagramme

- **Sequenzdiagramm**: Spieler-Interaktion → DialogWindow-Methoden → Interaktionsende
    
- **Klassendiagramm**: Beziehungen zwischen DialogInteractable, DialogFactory, Dialog und DialogEntry
    

## Tests

1. **Erster Dialogstart**: Interaktion öffnet Dialogfenster mit erstem Eintrag
    
2. **Fortsetzen im laufenden Dialog**: erneute Interaktion führt zur Fortsetzung
    
3. **Dialogabschluss**: Nach letztem Eintrag schließt Fenster, Flag wird zurückgesetzt
    
4. **Zufallsauswahl**: Mehrfache Starts liefern variierende Dialoge
    
5. **Sprachfallback**: Fehlende Übersetzung → Fallback auf Englisch → alternativ erster Eintrag
    
6. **Abbruch**: UI vor Ende schließen → nächster Interaktionsversuch startet neu
    
7. **Server-Stub-Funktionen**: Aufruf von SERVER_Interact und SERVER_FinishInteraction ohne Fehler
    

## Offene Punkte / Probleme

- Fehlende Server-Synchronisation kann zu Desynchronisation führen
    
- Einfaches Flag-System skaliert schlecht bei parallelen Interaktionen
    
- Keine Laufzeit-Erweiterung oder Nachladung von Dialogdaten möglich