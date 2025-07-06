# TLDR  
Das erweiterte Matchmaking-System ersetzt klassische Lobbies durch eine automatische Spielersuche mit optionaler Gruppenintegration, fokussiert auf schnelle Koop-Erlebnisse. Das zugehörige UI-Fenster zeigt Statusinformationen und erlaubt Abbruch des Vorgangs.

## Zweck / Ziel  
Das System wurde entwickelt, um den Matchmaking-Prozess im Coop-Modus effizient, wartungsarm und für Spieler intuitiv zu gestalten. Statt klassischer Lobbies, in denen aktiv nach Spielen gesucht wird, läuft der Verbindungsprozess automatisch im Hintergrund. Ziel ist es, Spieler auch ohne Freunde oder soziale Verbindungen zusammenzuführen und dabei die Wartezeit so gering wie möglich zu halten. Das `UIMatchmakingWindow` dient als Feedback-Komponente für diesen Ablauf.

## Umsetzung  

### Relevante Kooperationspartner (Klassen/Dateien)  
- `DilibadMatchmakerModule`: Verwaltet Server-seitige Spielersuche und Session-Erstellung  
- `DilibadMatchmakerClient`: Bindeglied zwischen Client und Server für Matchmaking-Abläufe  
- `QueuedPlayer`, `EnqueueInformation`: Datenstruktur zur Warteschlange und UI-Darstellung  
- `UIMatchmakingWindow`: UI-Komponente für Rückmeldung und Interaktion während der Suche  
- `MatchmakingOpCodes`: Opcode-Definitionen für Client-Server-Kommunikation

### Ablauf und Interaktionen (Unity & Networking)  
1. Spieler initiiert Matchmaking über Button → `DilibadMatchmakerClient.RequestSoloMatch()`  
2. Server prüft, ob laufende Session verfügbar ist. Wenn nein, wird Spieler der `soloQueue` hinzugefügt.  
3. Server sendet `QueueJoined` inklusive `EnqueueInformation`.  
4. `UIMatchmakingWindow.Show()` wird aufgerufen und zeigt:
   - den Namen des Spielmodus (über `GameLoopConfigTemplate`)
   - die vergangene Wartezeit (in `Update()` mit Differenz zu `EnqueueTime`)  
5. Bei Match-Fund: Server sendet `FoundMatch`, Session wird geladen  
6. Spieler kann jederzeit über Button `CancelMatchmaking()` abbrechen → Server entfernt Spieler aus Queue  

### Architektur- oder Designentscheidungen  
- **Keine Lobbies**: Keine manuelle Spielwahl notwendig, Fokus auf sofortiges Spiel  
- **Abbruch jederzeit möglich**: UI erlaubt expliziten Abbruch mit garantierter Queue-Entfernung  
- **Live-Feedback**: `UIMatchmakingWindow` zeigt Status visuell und in Echtzeit  
- **Kein Scroll- oder Auswahlsystem im UI**: Moduswahl erfolgt vorher, Fenster zeigt nur aktuellen Fortschritt  
- **Schlanker Ablauf**: `UIMatchmakingWindow` ist rein anzeigend und delegiert alle Logik an Services  
## Tests  
### Manuelle Prüf- und Verifikationsschritte  
- [ ] Klick auf Start-Button öffnet `UIMatchmakingWindow`  
- [ ] Spielmodusname im Fenster korrekt angezeigt (laut `GameLoopConfigTemplate`)  
- [ ] Wartezeit läuft korrekt hoch (Vergleich zur Systemzeit)  
- [ ] Klick auf Abbrechen-Button führt zum Senden von `RequestLeaveQueue`  
- [ ] Spieler erhält nach Abbruch `QueueLeft`  
- [ ] Bei Match-Fund wird Fenster automatisch geschlossen (anwendungsspezifisch testen)  
- [ ] Falsche oder unbekannte Spielmodi führen nicht zu Absturz  
- [ ] Nach Wiederholung der Suche wird neues Fenster korrekt initialisiert  

## Offene Punkte / Probleme  
- Keine explizite Animation oder Ladeindikator im UI  
- Fenster zeigt keine Statusmeldungen bei Netzwerkproblemen  
- Kein automatischer Retry bei Verbindungsverlust  
- Match-Fund wird nicht direkt im UI behandelt (kein Success-Overlay, keine Statusmeldung)  
- `UIMatchmakingWindow` ist rein statusanzeigend – kein Fortschrittsbalken oder Gruppenzusammensetzung sichtbar  
