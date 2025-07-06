# TLDR

Steuert zentralisiert die Sichtbarkeit und den Ablauf der UI-Elemente im Hauptmenü, entkoppelt von den einzelnen UI-Komponenten und gewährleistet konsistente Zustandsübergänge.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um die Logik zur Anzeige und zum Verbergen der verschiedenen Hauptmenü-Elemente (Login-Screen, Initial-Load-Gates, Main Menu, Party, Chat, Matchmaking) an einer zentralen Stelle zu bündeln. Es löst das Problem, dass einzelne UI-Elemente selbst entscheiden müssten, wann sie angezeigt werden, und sorgt so für eine klare Trennung zwischen Anwendungszustandsverwaltung und UI-Darstellung.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **ApplicationStateService**: Verwaltet den globalen Anwendungszustand, reagiert auf Netzwerk- und Auth-Events und ruft den Menü-Flow auf.
        
    - **ApplicationMainMenuFlow**: Führt basierend auf aktuellen Zuständen (Offline, Loading, MainMenu, InGameSession) spezifische UI-Aktionen durch.
        
    - **ServiceLocator.UIService**: Stellt die Methoden zum Spawn, Show und Hide der jeweiligen UI-Fenster bereit.
        
    - **GameNetworkService & Mst.Client**: Signalisieren Verbindungs- und Auth-Statusänderungen, auf die der Flow reagiert.
        
    - **Unity SceneManager**: Löst bei Szenenwechseln „Client“ das erneute Ausführen des Flows aus.
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **WarmUp**: `ApplicationStateService` initialisiert den Zustand auf Offline und erstellt `ApplicationMainMenuFlow`.
        
    2. **Start**: Übergang auf MainMenu-Zustand; Registrierung aller relevanten Events (ConnectionStatus, Auth, ClientConnected/Disconnected, Szene geladen).
        
    3. **Event-Handling**: Bei jeder Statusänderung ruft `ApplicationStateService` `_menuFlow.Process()` auf.
        
    4. **Process-Logik** (`ApplicationMainMenuFlow.Process`):
        
        - Ignoriert Loading- und InGameSession-Zustände.
            
        - Für Kombinationen aus Netzwerk- und Auth-Flags werden gezielt UIService-Aufrufe getriggert, um die passenden Fenster zu zeigen oder zu verbergen (z. B. LoginScreen.Show/Hide, InitialLoadWindow.OpenGates, UIMainMenu.Show).
            
        - Nutzt Callback `OnGatesOpened`, um nach dem Öffnen der Loading-Gates das Hauptmenü freizugeben.
            
- **Architektur- oder Designentscheidungen**
    
    - **Entkopplung**: Trennung von Anwendungszustandslogik und UI-Darstellung über einen dedizierten Flow.
        
    - **Ereignisgesteuert**: Reaktive Reaktion auf Netzwerk- und Auth-Ereignisse statt Pull-Logik in UI-Elementen.
        
    - **ServiceLocator-Muster**: Ermöglicht lose Kopplung und leichte Austauschbarkeit von UI- und Netzwerkschichten.
        
    - **Keine direkten UI-Referenzen** in `ApplicationStateService`: Sämtliche UI-Aufrufe laufen über `ApplicationMainMenuFlow`.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Komplexität vs. Wartbarkeit**: Externe Flow-Klasse erhöht Anzahl der Komponenten, verbessert aber Übersichtlichkeit.
        
    - **Event-Order-Risiko**: Abhängigkeit von korrekter Reihenfolge bei Status-Events kann zu kurzzeitigen UI-Flackern führen.
        
    - **DevMode-Ausnahmen**: Spezielle Pfade für Editor-Modus müssen separat getestet und dokumentiert werden.
        


## Tests

1. **Offline/DevMode-Szenario**
    
    - App starten ohne Main-Server-Verbindung im Editor → InitialLoadWindow öffnet sich, LoginScreen bleibt verborgen.
        
2. **Erste Verbindung**
    
    - Nach Verbindungsaufbau → LoginScreen wird angezeigt; bei DevMode direkt Login überspringen und Main Menu öffnen.
        
3. **Authentifizierung**
    
    - Erfolgreiches Login → InitialLoadWindow.Gates öffnen → Main Menu, Party, Chat, Matchmaking sichtbar.
        
4. **Szene wechseln**
    
    - SceneManager lädt „Client“ neu → `Process()` wird erneut aufgerufen und UI korrekt neu aufgebaut.
        
5. **Logout / Disconnect**
    
    - Netzwerkunterbrechung → Flow leitet zurück auf MainMenu-Zustand, LoginScreen erscheint.
        
6. **Re-Login nach erstem Laden**
    
    - Sicherstellen, dass `_firstLoad` false ist und Login-Pfade übersprungen werden.
        

## Offene Punkte / Probleme

- **Fehlendes Error-Handling** bei Auth-Misserfolg (kein Retry, kein Fehlerdialog).
    
- **Race Conditions** zwischen ConnectionStatusChanged und OnSignedInEvent können UI-Inkonsistenzen verursachen.
    
- **Unit Tests**: Automatisierte Tests für Flow-Logik fehlen – nur manuelle Tests vorhanden.
    
- **Skalierbarkeit**: Bei weiteren UI-Fenstern könnte `Process()` sehr lang werden; Überlegung: Modularisierung in Unterflows.
    

ChatGPT fragen