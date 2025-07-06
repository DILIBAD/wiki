# Topic Name  
GameSession Server Erweiterung

# TLDR  
Diese Komponente erweitert die Funktionalität eines GameSession-Servers innerhalb des MST-Frameworks, um Raumregistrierung und Spielerzählung automatisch zu verwalten.

## Zweck / Ziel  
Die Erweiterung dient dazu, GameSession-spezifische Logik beim Starten eines Spielraums und bei Änderungen der Spieleranzahl zentral zu verwalten. Sie sorgt für einheitliche Initialwerte und aktualisiert relevante Raumoptionen dynamisch während der Laufzeit.

## Umsetzung  
- **Relevante Kooperationspartner**  
  - `RoomServerManager`: Hauptverantwortlich für Spielraum-Verwaltung.  
  - `RoomController`: Steuerung des spezifischen Raums.  
  - `Mst.Server.Rooms`: Globale Raumverwaltung des Master Server Toolkits.  
  - `CustomProperties`: Sammlung benutzerdefinierter Raumoptionen.

- **Ablauf und Interaktionen (Unity & Networking)**  
  - Bei Aktivierung (`Start`) werden Event-Listener für Raumregistrierung sowie Spielerbeitritte und -verlassen registriert.  
  - `OnRoomRegistered` initialisiert bestimmte Raumoptionen wie Offenheit, Hilfebedarf und maximale Spieleranzahl.  
  - Bei Spielerbeitritt oder -verlassen wird die Anzahl aktiver Spieler gezählt und in den Raumoptionen aktualisiert. Diese Optionen werden anschließend im Server gespeichert.

- **Architektur- oder Designentscheidungen**  
  - Trennung von Raumlogik und eigentlicher Spiel-Session durch klare Verantwortung im `RoomServerManager`.  
  - Die Klasse agiert als Vermittler zwischen Mirror-Instanz und MST-Backend.  
  - Direkte Nutzung von `Mst.Server.Rooms.SaveOptions(...)` zur Synchronisierung der Raumkonfigurationen mit dem Master Server.  
  - Fallback-Sicherheit durch Prüfung auf Vorhandensein von `RoomServerManager` bei `Awake`.

- **Reflektion: wichtige Überlegungen und Trade-offs**  
  - Die Events werden bewusst lokal verwaltet (nicht persistent über Szenen hinweg), was den Server leichter wartbar macht.  
  - Kein direkter Bezug auf Spielmodi oder zusätzliche Logiken; Erweiterbarkeit über `CustomProperties` möglich.  
  - Potenzielle Race-Conditions bei gleichzeitigen Spielerbeitritten werden implizit durch Events behandelt, jedoch nicht explizit abgesichert.


## Tests  
Manuelle Testfälle zur Verifikation:

1. **Raumregistrierung**
   - Starte einen GameSession-Server.
   - Überprüfe, ob die Konsole die korrekte RoomId ausgibt.
   - Öffne das MST-Dashboard und validiere die gesetzten Raumoptionen (`IsOpen`, `IsHelpNeeded`, `RoomId`, `RoomMaxConnections`).

2. **Spielerbeitritt**
   - Tritt dem Raum mit einem Client bei.
   - Prüfe, ob `OnlineCount` erhöht wird.
   - Bestätige Aktualisierung im MST-Dashboard.

3. **Spielerverlassen**
   - Trenne den Client.
   - Prüfe, ob `OnlineCount` entsprechend reduziert wurde.
   - Validierung im MST-Dashboard.

4. **Fehlerszenario**
   - Entferne `RoomServerManager` aus dem GameObject und starte neu.
   - Überprüfe, ob der Debug-Fehler korrekt ausgegeben wird.

## Offene Punkte / Probleme  
- **Fehlende Fehlerbehandlung in SaveOptions-Callbacks**: Erfolg und Fehlerfälle werden ignoriert.  
- **Keine Validierung bei Mehrfachbeitritten / Ghost-Clients**: Dies kann zu inkonsistenten Spielerzählungen führen.  
- **Event-De-Registrierung nur teilweise**: Nur `Mst.Server.Rooms.OnRoomRegisteredEvent` wird bei `OnDestroy` entfernt, die UnityEvents des `roomManager` bleiben registriert.
