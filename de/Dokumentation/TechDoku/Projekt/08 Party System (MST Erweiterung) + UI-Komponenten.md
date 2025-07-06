
# TLDR  
Das Party-System erlaubt es Nutzern, Partys zu erstellen oder beizutreten, und zeigt offene Partys in einem UI an. Begleitende UI-Elemente unterstützen Benutzerinteraktion, Mitgliederanzeige und Partyverwaltung.

## Zweck / Ziel  
Das System soll die Gruppierung von Spielern zu Partys erleichtern – als Vorbereitung auf kooperatives Spiel oder Matchmaking. Die dazugehörige UI zeigt offene Gruppen an, erlaubt einfache Verwaltung der eigenen Party und visualisiert Mitgliederinformationen und Bereitschaftszustände.

## Umsetzung  

### Relevante Kooperationspartner (Klassen/Dateien)  
- `UIPartyWindow`: zentrales UI-Fenster zur Partyverwaltung  
- `UIPartyPromotionWindow`: Anzeige offener Partys (Promotion UI)  
- `UIPartyMemberDisplayElement`: Darstellung einzelner Party-Mitglieder  
- `UIAccountDisplayElement`: Darstellung eigener Accountinformationen  
- Interagiert mit `ServiceLocator.PartyService` für Backend-Operationen  

### Ablauf und Interaktionen (Unity & Networking)  

#### `UIPartyWindow`  
- Zeigt eigene Partyinformationen an (Party-ID, Mitgliederliste)  
- Ermöglicht:  
  - Party-Erstellung (`_createParty`)  
  - Beitritt per Party-ID (`_joinParty`)  
  - Verlassen der Party (`_leaveParty`)  
  - Kopieren der Party-ID in die Zwischenablage (`_copyPartyIdButton`)  
- Nutzt `UIPartyMemberDisplayElement` zur Darstellung anderer Mitglieder  
- Eigene Profilinformationen werden über `UIAccountDisplayElement` geladen  
- Reagiert auf Events des `PartyService` wie `OnPartyJoined`, `OnPartyLeft`, `OnPartyMembersChanged`

#### `UIPartyPromotionWindow`  
- Wird nach erfolgreichem Login automatisch eingeblendet  
- Zeigt Informationen zur aktuell beworbenen Party (`PromotedParty`)  
  - ID, Modus (z. B. PvE, PvP), Anzahl Mitglieder  
- Buttons:  
  - Beitreten (`_joinButton`) → beitritt zur gewählten offenen Party  
  - Weiter (`_passButton`) → nächste offene Party laden  
- Greift auf zentralen `PartyService` zur Anzeige und Navigation durch verfügbare Gruppen zu  

#### `UIPartyMemberDisplayElement`  
- Zeigt einzelne Party-Mitglieder mit:  
  - Anzeigename  
  - Level  
  - Ready-Status (aktiviert ein visuelles Objekt)  
- Lädt Profilinformationen dynamisch über `Mst.Client.Profiles.FillInProfileValues`  
- Visuell einfach gehalten, aber erweiterbar (z. B. Profilbilder)  

#### `UIAccountDisplayElement`  
- Zeigt Spielername und Level des eingeloggten Benutzers  
- Nutzt `ObservableProfile`, um zentrale Daten asynchron zu laden  
- Wird vom Party-Hauptfenster bei `Show()` initialisiert  

### Architektur- oder Designentscheidungen  
- Trennung von UI-Komponenten nach Funktion (Promotion vs. Management)  
- Verwendung von Events statt Polling zur UI-Aktualisierung  
- Dynamisches Laden von Profilinformationen aus dem Netzwerk bei Bedarf  
- Kompaktes, mobiles UI-Design mit klaren Zustandsübergängen  
- Wiederverwendbare Komponenten für Party-Mitgliederanzeige  

### Reflektion: wichtige Überlegungen und Trade-offs  
- Bewusste Reduktion auf wesentliche UI-Elemente für Übersichtlichkeit  
- UI ist abhängig von der vollständigen Funktionalität des PartyService – Offline-Modus nicht vorgesehen  
- Keine Drag&Drop- oder Sortiermöglichkeiten für Mitglieder  
- Ready-State nur visuell erkennbar, keine Interaktion darüber möglich  

## Screenshots / Diagramme  
- Vorschläge:  
  - `UIPartyWindow` mit Party-Infos, Mitgliedsliste und Buttons  
  - `UIPartyPromotionWindow` mit angezeigter Party und Aktionsbuttons  
  - Mockup eines `UIPartyMemberDisplayElement` mit Ready-Status-Anzeige  
- Zustandsdiagramm:  
  - User nicht in Party → `Create` oder `Join`  
  - User in Party → Anzeige Mitglieder, Leave möglich  
  - UI wechselt abhängig von Events (`OnPartyJoined`, `OnPartyLeft` usw.)  

## Tests  

### Manuelle UI-Prüfungen  
- [ ] `UIPartyWindow` öffnet korrekt und zeigt keine Party bei Start  
- [ ] `CreateParty` erstellt neue Party und aktualisiert Anzeige korrekt  
- [ ] Mitgliederanzeige wird korrekt aktualisiert bei Join/Leave/Ready-Up  
- [ ] Ready-State-Anzeige schaltet zuverlässig bei Statusänderungen  
- [ ] Beitritt über Party-ID-Feld funktioniert mit gültiger ID  
- [ ] Kopieren der Party-ID über Button funktioniert  
- [ ] `UIPartyPromotionWindow` wird nach Login angezeigt  
- [ ] `Pass`-Button zeigt nächste offene Party  
- [ ] `Join`-Button tritt korrekter Party bei  
- [ ] Fehlerfälle (ungültige ID, keine offenen Partys) verhalten sich stabil  
- [ ] Profilinformationen (Name, Level) werden korrekt geladen und angezeigt  
- [ ] UI bleibt konsistent nach Disconnect oder Partyauflösung  

## Offene Punkte / Probleme  
- Kein UI zur Verwaltung der Leader-Rolle oder zum Kick von Mitgliedern  
- Kein visuelles Feedback bei Fehlern (z. B. Beitritt fehlgeschlagen)  
- Partygröße ist fix, keine UI zur Konfiguration von MaxSize  
- Profilbilder sind vorgesehen, aber noch nicht implementiert (TODO)  
- Es fehlt ein Ladezustand oder visuelles Feedback bei Netzwerkverzögerungen  
