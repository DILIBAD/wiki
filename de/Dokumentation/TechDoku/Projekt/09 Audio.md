# TL;DR

Das Sound-System sorgt für gezieltes akustisches Feedback im Spiel. Es umfasst  

Hintergrundmusik, Umgebungsgeräusche, UI-Sounds und Interaktionsgeräusche.  

Ziel ist es, Atmosphäre zu erzeugen, Aktionen verständlich zu machen und die Immersion zu erhöhen.

  
## Zweck / Ziel

Das Sound-System soll verschiedene Aspekte des Spiels akustisch unterstützen:  

- **Feedback:** Aktionen der Spieler (z. B. Button klicken, Mining) erhalten klare akustische Rückmeldungen.  

- **Atmosphäre:** Hintergrundmusik und Umgebungssounds erzeugen Stimmung und stärken die emotionale Bindung.  

- **Spielstruktur:** Sounds dienen der Orientierung, etwa durch Hinweistöne beim Öffnen eines Menüs oder beim Beginn einer neuen Wave.  

- **Motivation:** Erfolgs-Sounds (z. B. bei Questabschluss) steigern die Belohnungswahrnehmung.

  

## Umsetzung

### Wichtigste Klassen / Dateien

- `AudioConfigTemplate.cs` – ScriptableObject mit allen Audio-Kategorien, Clips und zugehörigen Mixer-Gruppen  

- `AudioService.cs` – Zentrale Verwaltung von SFX, Musik, One-Shots und Looping-Themes; inkludiert Pooling und Context-Management  

- `AudioKeys.cs` – Statische Definition aller Sound-Keys zur Vermeidung von Magic Strings  

- `AudioInstance.cs` – Singleton-Behaviour mit DontDestroyOnLoad zum Persistieren von AudioSources  

- `AudioUIEmitter.cs` – Komponente für UI-Elemente, die bei Interaktion One-Shot-Sounds auslöst  

- `AudioLevelTheme.cs` / `AudioLevelEffect.cs` – Komponenten, die beim Szenenstart passende Themes oder Effektsounds triggern  

- **Editor-Tools:**  

  - `AudioKeyViewerWindow.cs` – EditorWindow zur Anzeige und Filterung aller AudioKeys per Reflection  

  - `AudioConfigEditorWindow.cs` – Custom Inspector für das AudioConfigTemplate mit Such- und Drag’n’Drop-Funktion

  
### Ablauf / Interaktionen

1. **Initialisierung:**  

   - Beim Spielstart lädt der `AudioService` automatisch das konfigurierte `AudioConfigTemplate`.  

   - Zwei AudioSource-Instanzen werden vorgerendert und in einen Pool gelegt.  

2. **One-Shot-Playback:**  

   - Aufruf `PlayOneShot(key)` → Lookup in `AudioConfigTemplate` → AudioSource aus Pool holen → Clip zuweisen → Abspielen → nach Ende zurückgeben.  

3. **Theme-Wiedergabe:**  

   - Aufruf `PlayTheme(key)` → bisheriges Theme stoppen → neue, non-gepoolte AudioSource im Loop starten.  

4. **2D-/Context-Sounds:**  

   - `Play2D(clip, mixerGroup, loop, context)` ermöglicht selektives Stoppen über `StopAllInContext(context)`.  

5. **UI-Interaktionen:**  

   - `AudioUIEmitter` bindet UnityEvents an Buttons, um `PlayOneShot("ButtonClick")` aufzurufen.  

6. **Level-Start:**  

   - `AudioLevelTheme` und `AudioLevelEffect` lösen beim Betreten einer Szene automatisch ihre zugewiesenen Sounds aus.

  
### Besondere Entscheidungen

- **Pooling:** Vermeidet Laufzeit-Instanziierung von AudioSources und reduziert Garbage.  

- **Context-Map:** Selektives Pausieren oder Stoppen von Sound-Gruppen (z. B. alle Treffer-SFX).  

- **ScriptableObject-Konfiguration:** Trennung von Daten und Code, einfache Pflege im Editor.  

- **Reflection in Editor-Tools:** Automatisches Auslesen aller Keys ohne manuelles Synchronisieren von Strings.  

- **ServiceLocator-Pattern:** Entkoppelte globale Zugänglichkeit des `AudioService`.  

- **UniTask vs. Coroutine:** Asynchrone Rückgabe ins Pool per UniTask („kleiner Footprint”) mit Fallback auf CoroutineRunner.

  

## Reflektion

Das Sound-System verbessert die Spielerfahrung spürbar, auch ohne aufwändige Voiceover-Integration.  

- **Stärken:**  

  - Klare akustische Rückmeldungen  

  - Modularer, erweiterbarer Aufbau  

  - Einfache Konfiguration und Wartung über ScriptableObjects  

- **Herausforderungen:**  

  - Auswahl von Sounds, die nicht auf Dauer nerven (z. B. UI-Clicks)  

  - Lautstärke-Balancing bei vielen gleichzeitigen Quellen  

  - Fehlende Cross-Fades beim Theme-Wechsel  

  - Begrenzte 3D-/Spatial-Audio-Unterstützung

  


## Tests

- Systematisches Durchklicken aller UI-Elemente zur Überprüfung auf Soundfeedback  

- Szenenwechsel-Tests mit verschiedenen Themes (Base, World, Kampf)  

- Mehrfaches gleichzeitiges Auslösen von One-Shots zur Validierung des Pooling-Verhaltens  

- Lautstärke-Anpassungen via `SetMasterVolume` und individuelle Mixer-Regler  

- A/B-Tests unterschiedlicher Mining-Sounds für bestes Feedback

  

## Offene Punkte / Probleme

- Musikübergänge (z. B. Base → Bosskampf) noch nicht voll überblendet – Übergänge teils abrupt  

- Kontext-Musik für Wave-Spawns fehlt noch  

- Raumklang / Positions-Audio für 3D-Effekte nur rudimentär realisiert  

- Automatische Anpassung der Gesamtlautstärke bei vielen simultanen SFX könnte optimiert werden  

- Langfristig: Evaluierung von Wwise oder FMOD für erweitertes Adaptive Audio