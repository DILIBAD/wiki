# Upgrade-System

# TLDR  
Zentrales, generisches Framework zur Verwaltung und Anwendung von Upgrades auf unterschiedliche Entity-Typen (Charakter & Gebäude), das Code-Duplizierung minimiert.

## Zweck / Ziel  
Dieses Teilsystem stellt eine einheitliche Möglichkeit bereit, beliebige Objekttypen (z. B. Charaktere oder Gebäude) stufenbasierte Verbesserungen (“Upgrades”) zu erteilen. Es löst folgende Probleme:
- Vermeidung redundanter Logik für unterschiedliche Entity-Typen  
- Kapselung von Kosten- und Level-Berechnung  
- Klare Trennung von Anzeige- (Client) und Anwendungs-Logik (Server)  

## Umsetzung  
- **Relevante Kooperationspartner**  
  - `IUpgradeable<T>`: Interface zur Kennzeichnung auf Entities, die Upgrades anbieten  
  - `IUpgrade<T>`: Definition der Upgrade-Metadaten (Icon, Name, Beschreibung, Kosten-Logik) und der Schnittstellen für Client-/Server-Checks  
  - `UpgradeBase<T>`: Basisklasse als `ScriptableObject`, die Kosten-Skalierung, Lokalisierung und gemeinsame Funktionalität kapselt  
  - `GenericUpgrades`: Konkrete Implementierung für alle `IEntity`-Objekte  
  - `CharacterUpgradeComponent` & `BuildingUpgradeComponent`: Komponenten auf Netzwerk-Entities, die verfügbare Upgrades zusammenführen und bereitstellen  

- **Ablauf und Interaktionen (Unity & Networking)**  
  1. **Initialisierung**: Auf jedem `NetworkBehaviour` mit UpgradeComponent werden per Inspector referenzierte Upgrade-Assets geladen.  
  2. **Anzeige-Logik (CLIENT_CanUpgrade)**: Vor Darstellung im UI wird geprüft, ob Level < MaxLevel und ob der lokale Spieler die benötigten Ressourcen besitzt.  
  3. **Anwendungs-Logik (SERVER_Upgrade)**: Auf dem Server werden bei erfolgreichem Kauf statische Modifikatoren auf das Zielobjekt angewandt, Ressourcen abgebucht und das Upgrade-Level synchronisiert.  
  4. **Netzwerk-Synchronisation**: Änderungen an Level-Variablen und Stat-Komponenten werden via Mirror repliziert.  

- **Architektur- oder Designentscheidungen**  
  - **Interface-Basierte Entkopplung**: Trennung von Upgrade-Definition (`IUpgrade<T>`) und Anwendungsort (`IUpgradeable<T>`) erlaubt einfache Erweiterung auf neue Entity-Typen.  
  - **ScriptableObject als Daten-Container**: Wiederverwendbare Upgrade-Assets mit konfigurierbarer Kostenkurve, Lokalisierungsschlüsseln und Stat-Vorlagen.  
  - **Kosten-Skalierung via AnimationCurve**: Flexible Definition des Anstiegs der Upgrade-Kosten pro Level ohne Codeänderung.  
  - **Mirror-Netzwerk-Attribute**: Verwendung von `[Server]` zum Schutz der kritischen Upgrade-Anwendungsschritte.  

- **Reflektion: wichtige Überlegungen und Trade-offs**  
  - **Flexibilität vs. Komplexität**: Generischer Ansatz benötigt mehr Abstraktion (Interfaces, Generics), erhöht aber Wiederverwendbarkeit.  
  - **Performance**: Dynamische Kostenberechnung pro Anfrage kann Overhead verursachen, ist jedoch meist vernachlässigbar.  
  - **Fehlerbehandlung**: Aktuell minimal; Rollback-Mechanismen bei fehlgeschlagenem Kauf könnten ergänzt werden.  


## Tests  
1. **Verfügbare Upgrades laden**  
   - Entity mit UpgradeComponent in Szene platzieren  
   - Im Inspector prüfen, dass alle referenzierten Upgrade-Assets gelistet werden  
2. **Client-Prüfung (CLIENT_CanUpgrade)**  
   - Mit ausreichenden Ressourcen: Upgrade-Button aktiv  
   - Mit zu wenig Ressourcen: Button deaktiviert  
   - Bereits auf MaxLevel: Button ausgegraut  
3. **Server-Upgrade (SERVER_Upgrade)**  
   - Upgrade auslösen: Ressourcenabzug, Stat-Modifikation, Level-Inkrement prüfen  
   - Mehrfaches Auslösen bis MaxLevel: keine Überschreitung  
   - Ungültiger Aufruf (Level ≥ MaxLevel): kein Ressourcenabbuchung, kein Level-Anstieg  
4. **Netzwerk-Sync**  
   - Upgrade auf Server durchführen, auf Client beobachten: Level-Update und Stat-Anpassung repliziert  
   - Beobachter-Clients erhalten konsistente Daten  

## Offene Punkte / Probleme  
- **Rollback bei Teilfehlern**: Kein automatischer Rückabwicklung bei fehlgeschlagenen Teilschritten (z. B. wenn PurchaseService fehlschlägt).  
- **Fehlende Feedback-Mechanismen**: UI zeigt aktuell nur Aktivierung/Deaktivierung, keine detaillierten Fehlermeldungen.  
- **Erweiterbarkeit auf weitere Entity-Typen**: Prüfung, ob zusätzliche Adapterklassen oder generischere Komponenten nötig sind.  
- **Server-Authority–Locking**: Parallelisierungsprobleme bei gleichzeitigen Upgrade-Requests könnten Race-Conditions verursachen.  
