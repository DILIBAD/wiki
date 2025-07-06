# TLDR

Verwaltung der Multiplayer-Netzwerkverbindungen mittels Mirror Networking und Integration in MST (Master Server Toolkit) für Session-Management.

## Zweck / Ziel

Die Klassen DilibadNetworkManager und GameNetworkService verwalten die Netzwerkverbindungen für Multiplayer-Spiele. Ziel ist eine flexible Steuerung der Client-Server-Kommunikation, einschließlich Session-Management und Verbindungshandling, durch die Integration mit dem MST Framework.

## Umsetzung

### Relevante Kooperationspartner

- `DilibadNetworkManager`: Erbt von Mirrors NetworkManager und erweitert dessen Funktionen.
    
- `GameNetworkService`: Bindeglied zwischen MST und Mirror zur Verwaltung von Sessions und Client-Verbindungen.
    
- `MirrorNetworkServiceConfigTemplate`: ScriptableObject zur Konfiguration des NetworkManagers und Client-Server-Komponenten.
    

### Ablauf und Interaktionen

- Der `GameNetworkService` initialisiert eine Instanz des `DilibadNetworkManager` während seiner WarmUp-Phase.
    
- Client- und Serveraktionen werden über Events im `GameNetworkService` ausgelöst und durch entsprechende Methoden im `DilibadNetworkManager` abgearbeitet.
    
- Methoden wie `StartHost()`, `StopHost()`, `StartClient()` und `StopClient()` steuern den Verbindungsaufbau und -abbau.
    
- Durch MST wird geprüft, ob eine Spielsession existiert, um entsprechend einen `NetworkManager` zu initialisieren.
    
- Client-Verbindungen werden mittels der Methode `Connect(string address, ushort port)` aufgebaut.
    

### Architektur- oder Designentscheidungen

- Mirror wurde aufgrund der großen Community, guter Dokumentation und einfacher Integration gewählt.
    
- MST wurde für sessionübergreifendes Management integriert, wobei die Kopplung minimal gehalten wurde, um spätere Entkopplungen zu erleichtern.
    
- Events in `GameNetworkService` bieten eine flexible Erweiterbarkeit und ermöglichen einfache Anpassungen an neue Anforderungen.
    

### Reflektion: wichtige Überlegungen und Trade-offs

- Eine vollständige Entkopplung von MST wäre ideal, wurde aber aufgrund möglicher Stabilitätsprobleme auf einen späteren Refactoring-Termin verschoben.
    
- Die Nutzung von Events zur Kommunikation zwischen Klassen ermöglicht eine lose Kopplung und gute Wartbarkeit, erhöht jedoch die Komplexität im Debugging.
    
   

## Tests

- Start einer Singleplayer-Session durch Aufruf der Methode `ClientRequestSingleplayer()` und Überprüfung des Hosts.
    
- Start und Stopp einer Multiplayer-Session mittels `ClientRequestMatch()` und `ClientRequestDisconnect()`.
    
- Client-Verbindungstest durch Verwendung der Methode `Connect(address, port)` und Überprüfung der Verbindung und Fehlerbehandlung.
    
- Überprüfung, ob die UI korrekt auf Szenenwechsel (`OnServerSceneChanged`, `OnClientSceneChanged`) reagiert.
    
- Validierung der Events beim Auftreten von Netzwerkfehlern (`NetworkError`, `ClientTransportException`).
    

## Offene Punkte / Probleme

- Evaluierung der vollständigen Entkopplung von MST und möglichen Konsequenzen.
    
- Fehlertoleranz und Robustheit im Falle unerwarteter Client-Disconnects weiter verbessern.