# Localization­system

# TLDR

Das Lokalisierungssystem versorgt UI-Elemente zur Laufzeit mit textlichen Übersetzungen, ermöglicht das automatische Erfassen aller Schlüssel im Editor und verwaltet das Nachladen aktueller Übersetzionsdaten aus einem Backend.

## Zweck / Ziel

Das Subsystem wurde entwickelt, um mehrsprachige Benutzeroberflächen in Unity-Projekten zu realisieren und gleichzeitig den Workflow für Entwickler zu optimieren. Es löst folgende Kernprobleme:

- **Dynamisches Nachladen**: Übersetzungen werden zur Laufzeit sowohl aus lokalen Ressourcen als auch bei Bedarf aus einem zentralen API-Backend geladen und im Persistent-Ordner zwischengespeichert.
    
- **UI-Binding**: UI-Textkomponenten erhalten automatisch den jeweils aktuellen, lokalisierten String basierend auf einem Schlüssel.
    
- **Schlüsselverwaltung**: Im Editor werden automatisch alle verwendeten Lokalisierungs-Keys (ScriptableObjects & Konstanten) gesammelt und in eine JSON-Datei exportiert, um Übersetzungsstatus und Lücken transparent zu halten.
    

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **`LocalizationService` & `LocalizationWrapper`**: Kernlogik für Laden, Speichern und Zugriff auf Übersetzungsdaten; Event-Trigger bei Sprachwechsel.
        
    - **`LocalizedText`**: MonoBehaviour-Komponente, die ein `TextMeshProUGUI` bei Aktivierung automatisch an den Service bindet und bei Sprachwechsel den Text aktualisiert.
        
    - **`LocalizationKeys`**: Statisch gepflegte Sammlung aller verfügbaren Schlüssel in Form von `const string`-Feldern, geclustert nach UI-Bereichen.
        
    - **`LocalizationKeyExporter` (EditorWindow)**: Editor-Tool, das alle SCVs (ILocalizable ScriptableObjects) und Konstanten-Schlüssel per Reflection sammelt und fehlende Einträge in `all_keys.json` ergänzt.
        
- **Ablauf und Interaktionen**
    
    1. **WarmUp** (ServiceBase): Lädt verfügbare Sprachen aus `Resources/Localization`, setzt aktive Sprache aus PlayerPrefs oder Default, feuert `OnLanguageChanged` nach initialer Ressourcendaten-Ladung.
        
    2. **Hintergrund-Update**: Holt Versionsliste vom Backend, vergleicht mit lokal gespeicherter Version, lädt neue Übersetzungsdaten bei Bedarf herunter, speichert sie persistent und in Editor-Resources, lädt sie dann in den Dictionary-Cache.
        
    3. **UI-Binding**: Bei `OnEnable()` registriert `LocalizedText` einen Listener auf `OnLanguageChanged` und ruft `UpdateText()` auf, um den aktuellen Übersetzungstext anhand des `localizationKey` vom Service abzurufen und dem TMP-Element zuzuweisen.
        
    4. **Editor-Export**: Per Menüeintrag im Tools-Menü öffnet `LocalizationKeyExporter` ein Fenster, in dem alle verwendeten Keys gesammelt, mit bestehenden JSON-Einträgen abgeglichen und fehlende ergänzt werden.
        
- **Architektur- und Designentscheidungen**
    
    - **Service Locator**: Entkopplung der Lokalisierungslogik von Consumer-Komponenten über einen zentralen Zugriffspunkt (`ServiceLocator.LocalizationService`).
        
    - **Flaches JSON-Format**: Vereinfachtes Flat-JSON („Key“: „Value“), unterstützt durch `LocalizationWrapper`, um flexiblen Import aus API-Wrapper-JSON `{ "translations": {…} }` zu ermöglichen.
        
    - **Editor-Integration**: Automatische Synchronisation von Übersetzungsschlüsseln im Editor zur Vermeidung manueller Duplikate und Inkonsistenzen.
        
    - **Fallback-Strategie**: Offline-Modus über Ressourcen-Ordner und Fallback auf Schlüssel-Name, wenn keine Übersetzung gefunden wird.
        
- **Reflektion: Überlegungen und Trade-offs**
    
    - **Performance vs. Flexibilität**: Die Hintergrund-API-Abfragen können Asynchronität bieten, erhöhen jedoch Komplexität bei Fehler-Handling.
        
    - **Ressourcen- vs. Persistent-Laden**: Im Build spart Persistent-Ordner Bandbreite bei mehrfachen Starts, im Editor stets frische Ressourcen für schnelle Iteration.
        
    - **Schlüsselverwaltung**: Automatische Reflection erleichtert Konsistenz, erhöht aber Build-Zeit im Editor und setzt auf korrekte Attribut-Dekoration in Scripts.
        


## Tests

1. **Sprachwechsel im Play-Mode**
    
    - Projekt starten, Sprache im Menü ändern → Alle `LocalizedText`-Komponenten aktualisieren umgehend ihren Text.
        
    - Fallback testen: Ungültigen Schlüssel setzen → Schlüsselname bleibt im UI sichtbar.
        
2. **Initiale Ressourcen-Ladung**
    
    - PlayerPrefs leeren → WarmUp lädt Standard-Sprache aus Resources und gibt vorhandene Keys korrekt aus.
        
3. **Hintergrund-Update**
    
    - Lokale Version niedriger als API-Version simulieren → Neue JSON im Persistent-Ordner ablegen, Service lädt Daten und feuert `OnLanguageChanged` erneut.
        
    - API offline simulieren → Service überspringt Update, lädt ausschließlich aus Resources.
        
4. **Editor-Export**
    
    - Neue ILocalizable-Asset anlegen mit extra Keys → „Scan & Export Missing Keys“ ergänzt die JSON-Datei um diese Keys.
        
    - JSON bereits vollständig → Export läuft ohne Hinzufügen durch und gibt Log „0 neue Keys hinzugefügt“.
        
5. **Fehlerfälle**
    
    - Schreibrechte fehlen im Persistent-Ordner → Fehler-Log im Console, aber Application läuft weiter und nutzt Resources.
        
    - Malformed JSON im Persistent-Ordner → Service lädt Ressourcen-Variante und meldet Fehler-Eintrag.
        

## Offene Punkte / Probleme

- **Fehlende Key-Validierung im Build**: Annotation `[LocalizationKey]` nur im Editor aktiv; zur Build-Zeit fehlen Warnungen bei falschen Schlüsseln.
    
- **Keine Übersetzungs-Editor-Integration**: Werte in `all_keys.json` werden leer gelassen; Übersetzer-Workflow noch manuell.
    
- **Unzureichendes Error-Feedback**: Netzwerk-Fehler beim Download werden nur im Log ausgegeben, keine UI-Anzeige für Nutzer.
    
- **Performance bei sehr vielen Keys**: Große Flat-JSONs können Initial-Ladezeit erhöhen; Caching-Strategien könnten optimiert werden.