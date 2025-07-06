# TLDR  
Erweitertes Authentifizierungs-Framework für Unity, bestehend aus MST-Servermodulen, Mirror-Integration und einer UI-Login-Komponente zur Benutzeranmeldung mit temporären Zugangsdaten.

## Zweck / Ziel  
Das System erlaubt eine sichere, aber einfache Benutzer-Authentifizierung zur Spielzeit – als Vorstufe zu einem später geplanten SSO-Workflow mit Steam. Die `UILoginScreen`-Komponente ermöglicht es Nutzern, sich direkt aus dem Spiel heraus mit Benutzername und Passwort anzumelden und optional im Offline-Modus fortzufahren.

## Umsetzung  

### Relevante Kooperationspartner
- `UILoginScreen`: UI-Komponente zur Erfassung und Übermittlung von Login-Daten.
- `AuthBehaviour`: zentrale Klasse für MST-Login-Workflow (nicht enthalten, aber verwendet).
- `CustomAuthModule`: Servermodul zur Authentifizierung und Accountverwaltung.
- `MstMirrorAuthenticator`: Verbindung zu Mirror für Spielbeitritt nach erfolgreichem Login.
- `AccountService`: Clientseitiger Zugang zu Benutzerdaten.
- `ClientToMasterConnector`: Startet Verbindung zu Master-Server.

### Ablauf und Interaktionen

#### Benutzerinteraktion
- Benutzer geben Benutzernamen und Passwort ein, optional mit „Remember Me“-Flag.
- Der Button „Login“ löst `AuthBehaviour.SignIn()` aus, welcher `Mst.Client.Auth.SignIn()` aufruft.
- Die Eingaben werden verschlüsselt an den Server übertragen, dort verarbeitet und ggf. gespeichert.
- Bei Erfolg erfolgt ein Übergang zu `UIMainMenu`.
- Bei Verbindungsausfall kann mit „Play Offline“ ein lokaler Spielstart erfolgen, ohne Authentifizierung.

#### UI-Logik
- Panels (`_connectedPanel`, `_notConnectedPanel`) signalisieren Verbindungsstatus.
- Buttons starten Authentifizierung, Verbindung oder wechseln in Offline-Modus.
- Kein direktes UI-Feedback bei fehlgeschlagenem Login implementiert.

### Architektur- oder Designentscheidungen
- Strikte Trennung von UI (Loginmaske) und Logik (AuthBehaviour) erlaubt Austauschbarkeit und Tests.
- „Offline Play“-Funktion direkt integriert für lokale Entwicklungs- und Testphasen.
- Verbindung wird manuell über Retry-Button aufgebaut, was Entwicklung vereinfacht, aber UX limitiert.

### Reflektion: Wichtige Überlegungen und Trade-offs
- Fehlendes Fehlerfeedback im UI verhindert derzeit Rückmeldung bei ungültigen Logindaten.
- UI ist bewusst minimalistisch gehalten für Entwicklungszwecke, nicht produktionsreif.
- Die vollständige Steuerung durch `AuthBehaviour.Instance.SignIn(...)` ist zwar elegant, aber ein Blackbox-Risiko bei Debugging.

## Screenshots / Diagramme  
- Screenshot der Loginmaske mit Username-, Passwort- und RememberMe-Feldern.  
- Ablaufdiagramm: Button „Login“ → `AuthBehaviour.SignIn` → MST Auth → AccountService → Spielbeitritt.  
- Diagramm: `UILoginScreen` wechselt dynamisch zwischen `_connectedPanel` und `_notConnectedPanel`.

## Tests  

### UI-Funktionalität
1. **Verbindungsstatus-Anzeige:**
   - Verbindung zum Master-Server trennen → `_notConnectedPanel` muss sichtbar sein.
   - Verbindung aktiv → `_connectedPanel` aktiv.

2. **Login-Versuch:**
   - Gültige Kombination → Spieler wird weitergeleitet, `UIMainMenu` erscheint.
   - Ungültige Kombination → Kein UI-Feedback, Spiel bleibt im Login-Screen (Problem).

3. **„Remember Me“-Funktion:**
   - Aktivieren → Token wird gespeichert (prüfbar durch erneuten Start).

4. **Offline-Modus:**
   - Button „Play Offline“ → Verbindung wird geschlossen, Hauptmenü wird geladen.

5. **Retry-Funktion:**
   - Button „Retry“ → Verbindung wird erneut initiiert.

### Integrationstests
- Login über UI → Account muss auf Server erstellt oder erkannt werden.
- Token-basierte Anmeldung bei erneutem Start prüfen (RememberMe gesetzt).
- Fehlverhalten bei fehlerhaften Zugangsdaten (derzeit keine Rückmeldung sichtbar).

## Offene Punkte / Probleme  
- **Kein Nutzerfeedback bei Loginfehlern**: Fehlerhafte Anmeldungen (z. B. falsches Passwort) führen zu keinem sichtbaren Hinweis.
- **Keine Ladeanzeige / Button-Blockade**: Es kann zu Mehrfachauslösungen kommen.
- **Fehlende Validierung**: Keine Prüfung auf Mindestlänge oder leere Felder im UI.
- **Abhängigkeit von `AuthBehaviour`**: Diese Klasse steuert den Anmeldeprozess, ist aber nicht dokumentiert oder modularisiert.

