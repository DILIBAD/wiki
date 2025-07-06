# TLDR  
Ein zentrales, modular aufgebautes Framework zur Definition, Konfiguration und Ausführung des gesamten Spielablaufs auf Server- und Client-Seite – von WarmUp über Wellen und Bosskämpfe bis hin zum sauberen Beenden. Integriert Master Server Toolkit (MST) für Session-Synchronisation und bereitet extensible GameMode-Konzepte vor.

## Zweck / Ziel  
Dieser Topic fasst sämtliche Verantwortlichkeiten der Game-Loop-Infrastruktur zusammen:

- **Konfigurationsmanagement**  
  Bietet mit `GameLoopConfigTemplate` ein einheitliches ScriptableObject-Asset für alle Parameter eines GameModes (Wellenanzahl, Schwierigkeitskurven, Ressourcenspawns, Belohnungen, Boss-Trigger usw.).  
- **Ablaufsteuerung**  
  Der `GameLoopService` orchestriert in sequenziellen Phasen den Lebenszyklus (WarmUp → Danger-Phase → Wave-Loop → BossFight → EndGame) und stellt sicher, dass alle teilnehmenden Subsysteme synchron agieren.  
- **MST-Integration**  
  Synchronisiert Spielzustände und Session-Optionen (`IsOpen`, `AllowLateJoin`, `SessionState`) über Master Server Toolkit, verhindert Late-Joins nach Spielende und stellt Statistiken/CustomProperties bereit.  
- **Erweiterbarkeit**  
  Durch Verwendung von Interfaces und Partial-Klassen lässt sich das GameLoop-System relativ einfach um neue Modi (Endless Waves, PvP, Speedrun, Sandbox, Arena) und Datenquellen (JSON-Pipelines, Dashboard-Konfiguration) erweitern.

## Umsetzung

### Relevante Kooperationspartner  
- **Konfiguration**  
  - `GameLoopConfigTemplate` (ScriptableObjectCached<IGameLoopConfigData>)  
  - Interfaces wie `IGameLoopConfigData` zur Abstraktion  
  - Zusätzliche Config-Assets: `WaveRewardSettings`, `ResourceConfigTemplate`, `FactionConfigTemplate`, AnimationCurves für dynamische Skalierung  
- **Service-Lifecycle**  
  - `GameLoopService` als zentraler Coordinator  
  - Partial-Klassen zur thematischen Trennung:  
    - `.Configuration` (Laden & Validieren der Config)  
    - `.GameLoop.Lifecycle` (WarmUp-/Start-/Stop-Phasen)  
    - `.GameLoop.Wave` (Wellen-Logik, Rewards, Reset)  
    - `.BossFight` (Boss-Spawn, Kill-Tracking, EndGame-Transition)  
    - `.Hooks` (Client-Events, UI-Synchronisation)  
    - `.MST` (CustomProperties-Management, RoomOptions)  
    - `.Reporting` (Statistiken, Analytics)  
- **Subsysteme** (über ServiceLocator)  
  - `DangerService` (erzeugt Gefahren-Level, löst Wellenstart aus)  
  - `WaveService` (Steuerung der Wellenzyklen)  
  - `RespawnService` (Kristall-/Feind-Respawns)  
  - `FactionService` (Team-/NPC-Logik)  
  - `GlobalInventory` & `WorldService` (Loot-Verteilung)  
  - `SpawnService` (Pool-Management)  

### Ablauf & Interaktionen  
1. **WarmUp**  
   - GameLoopService liest MST-Argumente (GameMode-Hash) und lädt das passende ScriptableObject.  
   - Setzt MST-RoomOptions (`IsOpen = true`, `AllowLateJoin = true`, `SessionState = WarmUp`).  
   - Startet einen Countdown via CancellationToken.  
2. **Danger-Phase**  
   - `DangerService.Tick()` erhöht Value basierend auf Zeit und Spielern.  
   - Bei Überschreiten des Schwellenwerts:  
     - `OnDangerLevelFull` wird ausgelöst → Wechsel in Wave-State.  
3. **Wave-Loop**  
   - **Pre-Wave**: Loot-Drops (MonsterDrops) werden in der Welt platziert.  
   - **Wave-Start**: Gegner spawnen entsprechend Config-Parametern.  
   - **Wave-Ende**:  
     - Alle Wellen-Gegner besiegt? → Rewards in `GlobalInventory` transferieren.  
     - `DangerService.Reset()`, Respawns durchführen (Kristalle, Feinde), dann `EnterBetweenWaves()`.  
   - Loop bis alle konfigurierten Wellen abgearbeitet.  
4. **BossFight**  
   - Erreichen des `BossThreshold` löst Boss-Spawn aus.  
   - Nach Boss-Tod: `_bossKills` inkrementieren, bei Erreichen von `TotalBosses` → EndGame.  
   - Sonst: Danger reset, ggf. neue Phase.  
5. **EndGame & Reporting**  
   - Aggregate-Spielstatistiken ermitteln (Spielzeit, Wellen geschafft, Spieler-Scores).  
   - MST-CustomProperties aktualisieren (`AllowLateJoin = false`, `SessionState = EndGame`, `EndGameResult`).  
   - Analytics-Events (z. B. über Reporting-Modul) auslösen.  
6. **Client-Sync**  
   - Hooks auf `OnGameModeStateChanged` und `OnEndGameResult` aktualisieren UI-Elemente.  
   - Events werden über Networking-Layer (Mirror) gepusht.  
7. **Shutdown**  
   - Bei `Disconnect` oder `LastPlayerExit` prüft Service `OnlineCount`.  
   - Nach konfigurierter Verzögerung via CancellationToken `Stop()` aufrufen und Server-Instanz beenden.

### Architektur- & Designentscheidungen  
- **Partial Classes**  
  Erlauben klare Trennung nach Funktion, reduzieren Datei-Größe und erleichtern Navigation bei großer Codebasis.  
- **Direkte MST-Manipulation**  
  Ermöglicht deterministisches Debugging gegen potentielle Race-Conditions eines Event-Bus, auf Kosten lose­rer Kopplung.  
- **Interfaces & Caching**  
  Abstraktion der Config-Daten fördert Testbarkeit und Erweiterbarkeit; Caching minimiert ScriptableObject-Lade-Overhead.  
- **AnimationCurve für Skalierung**  
  Dynamische Anpassung der Schwierigkeit anhand Spielerzahl ohne Code-Änderung im Editor.  

### Reflektion & Trade-offs  
- **Monolithisch vs. Modular**  
  Zentraler Ablauf ist übersichtlich, erschwert aber Hinzufügen komplett neuer Modi ohne Refactoring.  
- **Event-Bus vs. Direktaufrufe**  
  Event-Bus würde lose Kopplung und bessere Austauschbarkeit bieten, erhöht jedoch Komplexität beim Debuggen von Zustandsübergängen.  
- **Editor-Assets vs. Runtime-Daten**  
  ScriptableObjects sind Editor-freundlich, aber JSON-basiertes Nachladen und Dashboard-Steuerung fehlen bisher.

## Tests  
1. **Asset-Erstellung & -Laden**  
   - Neues `GameModeConfig` anlegen, im Editor befüllen, beim Start prüfen, ob `WarmUp()` das richtige Asset lädt.  
2. **WarmUp-Timer**  
   - Countdown ablaufen lassen, `SessionState` in MST auf EarlyGame überführen.  
3. **Gefahren-Trigger**  
   - DangerService manuell über Threshold treiben, verifizieren, dass `OnDangerLevelFull` feuert und Welle startet.  
4. **Wellen-Durchlauf**  
   - Simulation: Gegner spawnen, nach Defeat korrekte Reward-Vergabe, Danger-Reset, Respawns stattfinden.  
5. **BossFight-Logik**  
   - Mehrere Boss-Kills durchspielen, `_bossKills` zählen, EndGame-Übergang validieren.  
6. **MST-Optionen & LateJoin**  
   - In-Game und nach EndGame prüfen, dass `AllowLateJoin` und `IsOpen` korrekt toggeln.  
7. **Client UI Hooks**  
   - UI-Elemente abgleichen: Bei jedem State-Change und EndGame-Event korrekte Anzeige.  
8. **Disconnect / LastPlayerExit**  
   - Peer-Trennung erzwingen, `OnlineCount` beobachten, Anwendung schließt sich nach Delay.  

## Offene Punkte / Probleme  
- **Strikte MST-Kopplung**  
  Evaluieren, ob ein optionaler Event-Bus (Observer-Pattern) in Kombination mit Logging/Tracing Frameworks Debugbarkeit und Testbarkeit verbessert.  
- **GameMode-Erweiterung**  
  Implementierung von Basisklassen (z. B. `WaveBaseGameMode`) und Strategy-Pattern für Endless-, PvP- und Sandbox-Modi.  
- **Konfigurations-Modularität**  
  Prototyp: JSON-Dateien oder REST-Endpunkt zur Laufzeit einlesen, um neue Mode-Definitionen ohne Rebuild zu ermöglichen.  
- **Proof of Concept**  
  Detaillierte Konzepterstellung und Minimal-Implementierung eines datengesteuerten, plugin-fähigen GameLoop-Frameworks.  
