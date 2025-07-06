# Global Inventory System

# TLDR  
Zentralisierte Verwaltung und Synchronisation von global geteilten Ressourcen über Server und Client hinweg.

## Zweck / Ziel  
Dieses Teilsystem wurde entwickelt, um einen gemeinsamen Rohstoffpool bereitzustellen, in den alle Spieler Ressourcen einzahlen und aus dem sie entnehmen können. Es entkoppelt die globale Lagerlogik vom lokalen Inventar und stellt sicher, dass alle Teilssysteme konsistente Abfragen auf den globalen Bestand durchführen können.

## Umsetzung  
- **Relevante Kooperationspartner**  
  - ServiceLocator (Zugriff auf den Service und Lebenszyklus)  
  - `GlobalInventoryConfigTemplate` (Konfiguration des Inventory-Prefabs)  
  - `ItemContainer` / `IItemContainer` (gemeinsames Interface für Inventar-Container)  
  - Mirror-Netzwerkkomponenten (`SyncedItems`, OnStartServer/OnStartClient)  

- **Ablauf und Interaktionen**  
  1. **WarmUp** lädt die Prefab-Konfiguration aus dem ScriptableObject.  
  2. **ServerInitialize** spawnt das globale Inventory-Prefab über den Master-Server.  
  3. **OnStartServer** initialisiert den Container mit allen zulässigen Ressourcen (Zero-State).  
  4. **OnStartClient** registriert das lokale Container-Objekt beim Service und verknüpft Netzwerk-Events (`Add`, `Set`, `Remove`) mit internen Callback-Methoden.  
  5. Änderungen am Bestand lösen zentral das Event `OnInventoryUpdated` aus, sodass abhängige Subsysteme (UI, Wirtschaftssysteme) reagieren können.  

- **Architektur- oder Designentscheidungen**  
  - **Separates Service-Pattern**: Trennung von globaler Inventarlogik und lokaler Inventarverwaltung durch ein Interface (`IGlobalInventoryService`).  
  - **ScriptableObject-Konfiguration**: Austauschbarkeit des Inventory-Prefabs ohne Codeänderung.  
  - **Generische Interfaces**: Wiederverwendbarkeit für verschiedene Item-Typen bei gleichzeitigem Typ-Safety.  
  - **Event-Basierte Kommunikation**: Entkopplung von Geschäftslogik und Präsentation/Visualisierung.  

- **Reflektion: wichtige Überlegungen und Trade-offs**  
  - **Vorteil**: Klare Trennung von Zuständigkeiten, zentrale Fehlerbehandlung und Vermeidung von Duplikaten.  
  - **Nachteil**: Zusätzliche Abstraktionsschicht erhöht Komplexität und Potenzial für Synchronisations-Race-Conditions.  
  - **Alternative**: Direkte Integration in lokalem Inventar, aber dadurch weniger Flexibilität und höhere Kopplung.

## Screenshots / Diagramme  
- **Service-Lifecycle**: Diagramm der Phasen WarmUp → ServerInitialize → OnStartServer/OnStartClient → OnStop…  
- **Datenfluss**: Flowchart vom Netzwerk-Event bis zur Aktualisierung des globalen Containers und Auslösen von `OnInventoryUpdated`.  
- **Komponentenübersicht**: UML-Klassendiagramm mit ServiceLocator, ServiceBase, IItemContainer und GlobalInventoryService.

## Tests  
1. **Prefab-Spawn**  
   - Konfiguration ändern, Spiel starten → globales Inventory-Objekt erscheint im Netzwerk.  
2. **Server-Initialisierung**  
   - Prüfen, dass nach ServerStart alle definierten Item-Slots im Container existieren.  
3. **Client-Registrierung**  
   - Client beitritt → Service registriert Container, initiale Items werden per Event gemeldet.  
4. **Ressourcen hinzufügen/entfernen**  
   - Über Debug-UI Elemente hinzufügen und entfernen → `OnInventoryUpdated` feuert mit korrektem Index.  
5. **Synchronisation**  
   - Auf Server Items ändern → alle Clients erhalten korrekte Event-Callbacks.  
6. **Abmeldung**  
   - Client/Server stoppen → Service deregistriert und Events unsubscribed (keine weiteren Updates).

## Offene Punkte / Probleme  
- **Fehlende Clear-Event-Unterstützung**: Sammel-Operationen auf dem Container (z. B. Reset) lösen aktuell kein Update-Event aus.  
- **Logging**: Detaillierte Log-Ausgaben für Debugging fehlen in Release-Builds.  
- **Race Conditions**: Gleichzeitige Zugriffe auf den Container in kurzen Intervallen könnten zu Inkonsistenzen führen.  
- **Fehlerfallbehandlung**: Aktuell keine Rückmeldung, wenn das Prefab-Spawn scheitert oder der Container beim Registrieren null ist.

