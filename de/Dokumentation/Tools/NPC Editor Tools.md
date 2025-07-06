# NPC Editor Tools

# TLDR  
Eine Sammlung von drei Unity-Editor-Fenstern zum Bearbeiten und Validieren von NPC-Assets (Stats und Templates) über verschiedene Schwierigkeitsstufen hinweg.

## Zweck / Ziel  
Das „NPC Editor Tools“-Subsystem wurde entwickelt, um Game-Designer und Entwickler dabei zu unterstützen, NPC-Daten effizient und konsistent zu pflegen. Es löst folgende Probleme:  
- **Zentralisierte Bearbeitung** aller NPC-Statistiken und -Vorlagen für verschiedene Schwierigkeitsgrade.  
- **Batch-Modus** für Massenanpassungen ohne manuelles Öffnen einzelner Assets.  
- **Vollständige Übersicht** und Inline-Editing aller relevanten Parameter in einem eigenen Editor-Fenster.

## Umsetzung  

- **Relevante Kooperationspartner**  
  - `StatsBatchConfig` (liefert die Liste verfügbarer Schwierigkeitsstufen)  
  - `StatsDataTemplate` (enthält alle NPC-Basiswerte)  
  - `NPCEntityTemplate` (definiert Klassen-Icons, Animator Controller, Skills, States)  
  - Unity-Editor-API: `EditorWindow`, `AssetDatabase`, `SerializedObject`, `EditorGUILayout`

- **Ablauf und Interaktionen**  
  1. Beim Öffnen eines Fensters wird `StatsBatchConfig` gesucht und dessen Schwierigkeitsstufen geladen (Fallback auf Hard-Coded-Liste).  
  2. Asset-Database durchsucht alle NPC-Statistik- bzw. NPC-Template-Assets und sortiert sie nach Schwierigkeitsnamen.  
  3. Benutzer wählt per Tab die gewünschte Schwierigkeitsstufe.  
  4. In Scroll-Ansicht werden die Assets thematisch (z. B. nach Fraktion) gruppiert dargestellt.  
  5. Über Inline-Felder lassen sich Werte anpassen; per „Refresh Templates“ neu laden; per „Save All“ alle Änderungen in der Datenbank speichern.

- **Architektur- und Designentscheidungen**  
  - **Batch- vs. Voll-Editor**: Zwei spezialisierte Batch-Fenster (Stats / Entity) plus eine kombinierte Vollansicht, um je nach Use-Case Performance und Übersichtlichkeit zu optimieren.  
  - **Namensbasiertes Gruppieren**: Fraktions- oder Schwierigkeitszuordnung erfolgt über Zeichenketten-Parsing im Asset-Namen statt zusätzlicher Metadaten.  
  - **GUIStyles** für bessere Lesbarkeit und Farbkontraste im Pro- vs. Standard-Skin.

- **Reflektion: Wichtige Überlegungen und Trade-offs**  
  - **Performance vs. Echtzeit-Feedback**: Alle Assets werden beim Init geladen – zunächst synchron, um einfaches Caching und konsistente Anzeige sicherzustellen.  
  - **Namensabhängigkeit**: Parsing von Namen ist schnell, anfällig bei falscher Benennung. Alternative Metadaten‐Struktur würde Aufwand erhöhen.  
  - **Editor-Only**: Keine Laufzeit-Abhängigkeiten; Änderungen sind ausschließlich im Editor wirksam.

## Screenshots / Diagramme  
- **UI-Screenshot**:  
  - Darstellung der Tab-Leiste mit Schwierigkeitsstufen  
  - Scroll-Container mit gruppierten NPC-Einträgen  
  - Inline-Felder für Stats und Skills  
- **Ablaufdiagramm**:  
  1. Fenster öffnen → 2. Konfig laden → 3. Assets laden → 4. Darstellung & Bearbeitung → 5. Save All  
- **Datenflussdiagramm**:  
  - `StatsBatchConfig` → Tab-Initialisierung → `AssetDatabase` → `SerializedObject` → EditorGUI → Änderungen → `AssetDatabase.SaveAssets()`

## Tests  
1. **Fenster-Aufruf**  
   - Menüpunkt „Tools / Validator / Stats/NPC Batch Editor“ auswählen → Fenster öffnet sich ohne Fehlermeldung.  
   - Identisch für „Entity Batch Editor“ und „Full NPC Editor“.  
2. **Schwierigkeitsstufen-Ladefall**  
   - Mit vorhandenem `StatsBatchConfig` prüfen, ob alle konfigurierten Stufen als Tabs erscheinen.  
   - Datei temporär umbenennen → Fallback-Liste enthält erwartete Standardwerte.  
3. **Template-Refresh**  
   - Neue NPC-Assets hinzufügen, „Refresh Templates“ klicken → Einträge erscheinen korrekt unter der zugehörigen Stufe.  
4. **Gruppierung**  
   - Assets mit und ohne Unterstrich im Namen anlegen → Fraktionsüberschrift stimmt immer mit erstem Wort/Segment überein.  
5. **Inline-Bearbeitung**  
   - Werte in Stats- bzw. Entity-Feldern ändern → „Save All“ → Projekt neu laden → geänderte Werte persistent.  
6. **Save All & Refresh**  
   - Nach „Save All“ Projektfenster beobachten: keine Fehlermeldungen, AssetDatabase aktualisiert.  
   - UI-Skins testen: Pro vs. Standard → Farben der Header-Styles variieren korrekt.  
7. **Fehlerfälle**  
   - Keine NPC-Assets → Anzeige von Hinweistext und Aktivierung des „Neu Laden“-Buttons.

## Offene Punkte / Probleme  
- **Namenskonvention**: Strikte Übereinstimmung erforderlich; fehlkonfigurierte Assets werden nicht angezeigt.  
- **Performance bei großer Asset-Anzahl**: Initiales Laden kann spürbar verzögern.  
- **Keine Undo-Integration**: Änderungen via SerializedObject unterstützen derzeit kein Undo/Redo.  
- **Fehlendes Loggen**: Bei fehlerhaften Datenzugriffen (z. B. null-Referenzen) keine expliziten Fehlermeldungen im Editor-Log.  
- **Export/Import**: Kein Mechanismus zur Migration von Massenänderungen zwischen Projekten.
