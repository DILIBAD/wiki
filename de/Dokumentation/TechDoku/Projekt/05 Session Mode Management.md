# SessionModes Management

# TLDR  
Erweiterung des MST-Frameworks zur serverseitigen Verwaltung und clientseitigen Übergabe verfügbarer Spielmodi mittels ScriptableObjects – perspektivisch auch über Supabase steuerbar.

## Zweck / Ziel  
Dieses Subsystem verwaltet alle verfügbaren Spielmodi und stellt sie verbundenen Clients zur Verfügung. Ziel ist es, Clients frühzeitig über die verfügbaren Modi zu informieren, etwa zur Anzeige in Lobbys oder zur Modusauswahl.  
Ursprünglich war eine dynamische Verwaltung der Spielmodi über Supabase vorgesehen, was in späteren Projektphasen wieder aufgenommen werden soll. Aktuell erfolgt die Bereitstellung über lokal vorhandene `ScriptableObjects`.

## Umsetzung  

### Relevante Kooperationspartner
- `GameModeInfo` (Datenstruktur für Modus-Metadaten)
- `GameModesPacket` / `GameLoopConfigEntry` (Detailinformationen zu Modi)
- `SessionModesModule` (MasterServer-Modul zur Bereitstellung der Modi)
- `SessionModesClient` (Client-Komponente zur Abfrage)
- `SupabaseSessionModes` (optional: Supabase-Anbindung zur Live-Verwaltung)
- `GameModeEntry` (Datenbank-Modell bei Supabase)

### Ablauf und Interaktionen

#### Aktuelle Implementierung (lokale ScriptableObjects)
- Beim Serverstart lädt `SessionModesModule` alle verfügbaren `IGameLoopConfigData`-Instanzen aus den Unity-Ressourcen.
- Diese werden in `GameModeInfo`-Instanzen konvertiert und zwischengespeichert.
- Anfragen vom Client (OpCode `RequestGameModes`) werden mit den verfügbaren Modi beantwortet.

#### Client-Seite
- `SessionModesClient` registriert sich auf den OpCode und stellt eine Methode `RequestGameModes` zur Verfügung.
- Nach erfolgreicher Antwort werden die `GameModeInfo`-Daten übergeben und weiterverarbeitet.

#### Alternative Implementierung (Supabase – derzeit deaktiviert)
- `SupabaseSessionModes` baut Verbindung zu Supabase auf und lädt die Tabelle `game_modes`.
- Änderungen in der Datenbank (Insert/Update/Delete) werden über Realtime Channels verfolgt.
- Bei Änderungen wird das lokale Cache automatisch aktualisiert.
- Ziel: Modi ohne Client-Update aktivieren/deaktivieren oder bei Fehlern schnell ausblenden.

### Architektur- oder Designentscheidungen
- **Fallback auf lokale Ressourcen**: Aufgrund unzureichender Vorteile im frühen Projektstadium wurde Supabase deaktiviert.
- **Flexibler Wechsel möglich**: Struktur erlaubt problemlose Umschaltung zurück zu Supabase-Logik.
- **Datenrepräsentation**: Verwendung von `GameModeInfo` für Metadaten, `GameLoopConfigEntry` für vollständige Konfiguration.
- **Serialisierung via MST Properties**: Einheitliches Format für Netzwerkübertragungen.
- **Realtime-Verarbeitung mit Supabase**: Umsetzung über WebSocket-Listener ermöglicht schnelle Reaktion auf Änderungen.

### Reflektion
- Der initiale Aufwand für Supabase-Verwaltung war zu hoch im Vergleich zum Mehrwert.
- Die modulare Trennung erlaubt jedoch einen späteren, schrittweisen Ausbau.
- Realtime-Integration kann langfristig operative Prozesse (z.B. automatische Deaktivierung bei Fehlern) erheblich vereinfachen.

## Tests

### Manuelle Prüf- und Verifikationsschritte

#### Lokale Ressourcen
- [ ] Spielmodus als ScriptableObject erstellen und korrekt taggen (`IGameLoopConfigData`).
- [ ] Server starten und prüfen, ob Modus in `SessionModesModule` geladen wird.
- [ ] Client verbinden und `RequestGameModes` auslösen.
- [ ] Korrekte Anzeige/Verarbeitung der empfangenen Modi prüfen.

#### Supabase-Modus (zukünftig)
- [ ] Verbindung zu Supabase herstellen (z.B. Credentials korrekt).
- [ ] Änderungen in Tabelle `game_modes` führen zu Cache-Aktualisierung.
- [ ] Modi können in Supabase aktiviert/deaktiviert werden.
- [ ] Realtime Listener verarbeitet Insert/Update/Delete zuverlässig.
- [ ] Fehlerhafte JSON-Felder (z.B. Thresholds) verursachen keine Crashes.

## Offene Punkte / Probleme
- [ ] Supabase-Reaktivierung ist technisch vorbereitet, aber nicht aktiv.
- [ ] Keine Absicherung bei JSON-Feldfehlern in Supabase-Datenstruktur.
- [ ] Kein Mechanismus zur Validierung von ScriptableObject-Inhalten.
- [ ] Hash-Berechnung (`HashID`) aktuell nicht eindeutig überprüfbar.
- [ ] Kein zentrales Monitoring für verfügbare Modi oder Fehlerzustände integriert.
