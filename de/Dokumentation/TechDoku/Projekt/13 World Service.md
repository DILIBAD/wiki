# TLDR  
Koordinierung der Spielwelt inklusive Laden und Entfernen von Levels sowie Verwaltung des zugrunde liegenden Raster‐Bereichssystems.

## Zweck / Ziel  
Der World Service wurde entwickelt, um sämtliche dynamisch ladbaren Spielbereiche (Maps/Levels) zentral zu verwalten. Er sorgt für  
- Erzeugung und Vernetzung von Start-, explorablen und Boss-Levels  
- Verfolgung aktiver Maps und ihrer Verbindungen  
- Delegation des konkreten Spawns an die Level-Instanzen  
- Sicheres Freigeben von belegten Rasterflächen bei Despawn  
Damit löst er das Problem, dass verschiedene Subsysteme uneinheitlich Terrain und Spielbereiche anlegen und wieder entfernen müssten.

## Umsetzung  
- **Relevante Kooperationspartner**  
  - **WorldGridService** (Rasterverwaltung für Platzzuweisung und ­freigabe) 
  - **ServiceLocator** (Zugriff auf SpawnService, AssetNexusService, FactionService)  
  - **LevelEntity / ILevelEntity** (Inhaltliche Initialisierung und Portal-Handling)  
  - **MapConnection** (Kapselt Portal-Verknüpfungen zwischen Maps)  

- **Ablauf und Interaktionen**  
  1. **SetupWorld** lädt das festgelegte Startlevel und reserviert einen Bereich im Raster.  
  2. **SetupExplorableMaps** ermittelt Ausgangspunkte, wählt zufällig per Gewichtung weitere World- oder Fraktions-Levels aus, reserviert Platz, spawnt Instanzen und legt Portal-Verbindungen an.  
  3. **SpawnBossMap / SpawnFactionMap** nutzt die Raster­service-Schnittstelle für Platzallocation, initialisiert LevelEntity und registriert sie in internen Listen.  
  4. **DespawnMap / Server_Remove**: Entfernt MapConnection, gibt Rasterfläche frei, beendet Instanz und bereinigt interne Referenzen.  

- **Architektur-/Designentscheidungen**  
  - **Trennung von Logik und Rasterverwaltung**: WorldService kümmert sich nur um Semantic­Ebene, WorldGridService um physische Platzbelegung.  
  - **First-Fit-Allocationsstrategie**: Einfache, deterministische Platzwahl, ohne komplexe Compacting-Logik.  
  - **Delegation des Spawn-Flows**: LevelEntity übernimmt Details des eigentlichen Spawns und der Navigationseinrichtung (Portale, AI-NavMesh).  
  - **Exception-getriebene Fehlererkennung**: Bei fehlgeschlagener Platzzuweisung oder nicht auffindbaren Maps wird via Exception signalisiert.  

- **Reflektion: Wichtige Überlegungen und Trade-offs**  
  - **Performance vs. Fragmentierung**: Einfacher First-Fit verursacht potenzielle Fragmentierung, erschwert spätere Platzzuweisungen großer Maps.  
  - **Zentralisierte Kontrolle vs. Modularität**: Hohe Kopplung an ServiceLocator ermöglicht schnellen Zugriff, erschwert aber Unit-Tests ohne Mocking.  
  - **Fehlerbehandlung**: Exceptions geben eindeutige Fehlerursache, erfordern aber überall Callbacks oder Try-Catch bei Aufrufern.

## Screenshots / Diagramme  
- **Ablaufdiagramm „WorldService → WorldGridService → SpawnService → LevelEntity“**  
- **Datenflussdiagramm „Rasterbelegung und Freigabe“**  
- **UI-Mockup für Debug-Overlay: Belegte versus freie Zellen im WorldGrid**  

## Tests  
Manuelle Prüf- und Verifikationsschritte:  
1. **Start-Map laden**  
   - Welt initialisieren, prüfen, dass genau ein Startlevel gespawnt wird und im Raster markiert ist.  
2. **Explorable Maps hinzufügen**  
   - Verschiedene Werte für `maxExplorableMaps` testen, verifizieren, dass Anzahl der geladenen Maps ≤ Parameter ist.  
   - Überlappung im Raster verhindern (Debug-Overlay nutzen).  
3. **Fraktions- und Boss-Maps**  
   - Für eine definierte Fraktion Boss-Map spawnen, anschließend Despawn aufrufen und kontrollieren, dass Rasterfreigabe erfolgt und Referenz entfernt wird.  
4. **Portal-Verbindungen**  
   - Verbindungspunkte prüfen: Betreten eines Portals teleportiert an korrekt gesetzte Exit-Position.  
   - Mehrfaches Hinzufügen derselben Verbindung sollte eine Warnung auslösen, aber nicht duplizieren.  
5. **Randfälle**  
   - `maxExplorableMaps = 0` bzw. keine verfügbaren Templates: gilt als „kein Spawn“ und schreibt Warnungen.  
   - Raster vollständig belegt: `TryAllocateArea` schlägt fehl, Exception im SpawnFlow auslösen.  
6. **Fehlerzustände**  
   - DespawnMap ohne bestehende Verbindung: prüft Logging von Warnung und keine Ausnahme.  
   - Double-Free einer Rasterfläche: sollte keine Fehlzustände erzeugen.

## Offene Punkte / Probleme  
- **Fragmentierung des Rasters**: Kein Repacking vorhanden, kann langfristig zu Platzmangel führen.  
- **Fehlende Zeitübersicht**: Spawn-Zeitpunkte und Despawn-Zeitpunkte werden nicht protokolliert.  
- **Ungetestete Methoden**: `RespawnCrystals` und `RespawnEnemies` sind deklariert, aber nicht implementiert bzw. getestet.  
- **Race Conditions**: Gleichzeitiger Zugriff auf `_allocatedAreas` in Multithread-/Async-Umgebungen nicht abgesichert.  
- **Logging-Konsistenz**: Unterschiedliche Loglevels (Debug, Warning, Exception) könnten harmonisiert werden.
