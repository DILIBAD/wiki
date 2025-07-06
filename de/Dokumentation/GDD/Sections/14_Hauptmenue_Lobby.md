# Hauptmenü & Lobby-UI

## Überblick

Die Menü-UI bildet den Einstiegspunkt in das Spiel **„DILIBAD“** und ist zentral für die Navigation, das Matchmaking und die soziale Interaktion. Sie vereint ein **stimmungsvolles Pixel-Art-Design** mit klar strukturierten Interaktionsflächen. Ziel ist eine intuitive Benutzerführung und die optische Einarbeitung in die Spielwelt.

---

## Hauptmenüstruktur (Startscreen)

Beim Spielstart landet der Spieler im **Hauptmenü**, das folgende Kernbereiche bietet:

- **Spielmodi-Auswahl (links unten)**
- **Partysystem (unten mittig)**
- **Chatfenster (rechts)**
- **Einstellungen (unten rechts)**

**Designentscheidung:**  
Die visuelle Aufteilung erlaubt eine gleichzeitige Orientierung über Spielfunktionen, Multiplayer-Zugänge und soziale Komponenten. Das Artwork im Hintergrund verankert den Spieler sofort in der Welt von „DILIBAD“.

---

## Spielmodus-Auswahl

Durch Klick auf „**SEARCH MATCH**“ öffnet sich ein Dropdown-Menü mit folgenden Modi:

- **Campaign**
- **Hardcore**
- **Quickmatch**

Der zuletzt gewählte Modus bleibt hervorgehoben und ist direkt per Klick erneut auswählbar.

**Designentscheidung:**  
Das Dropdown reduziert UI-Clutter und erlaubt dennoch schnellen Zugriff. Die linke Platzierung trennt die Spielauswahl bewusst vom Party- und Chatbereich.

---

## Partysystem

In der unteren Mitte kann der Spieler:

- Eine **Party erstellen**
- Einer bestehenden Party per **Party-ID** beitreten
- Den **aktuellen Party-Status** einsehen (z. B. Mitgliederzahl über ein Icon)

**Designentscheidung:**  
Die zentrale Platzierung signalisiert die Bedeutung von Koop-Gameplay. Die Eingabe über Party-ID ermöglicht gezielte Gruppensuche, auch außerhalb automatisierten Matchmakings.

---

## Chatfenster

Der rechte Bereich des Bildschirms ist dem **globalen Chat („General“) gewidmet**.  

Funktionen:

- Nachrichten senden (Textfeld unten)
- Echtzeit-Status („Joining“, „Connected“, etc.)
- Mehrere Kanäle optional erweiterbar (zukünftig)

**Designentscheidung:**  
Die klare Trennung vom Spielfeld fördert Konzentration auf soziale Interaktion, ohne UI-Elemente zu überlagern. Schwarz als Hintergrundfarbe erhöht die Lesbarkeit bei farbiger Schrift.

---

## Einstellungen (Optionsmenü)

Zugänglich über das **Zahnrad-Symbol unten rechts**.

### Kategorien:
- **Game:** z. B. Hints aktivieren/deaktivieren
- **Graphics:** Auflösung und Detailgrad (z. B. „1920×1080 / Medium“)
- **Sound:** Audio aktivieren/deaktivieren, Lautstärkeregler
- **Language:** Sprachauswahl (derzeit: „english“)

Weitere Buttons:
- **Reset** (setzt alles zurück)
- **Safe Mode** (vermutlich für Diagnose-/Kompatibilitätsmodus)

**Designentscheidung:**  
Das Optionsmenü bleibt bewusst schlicht und kompakt, um nicht vom Stil der Pixel-Ästhetik abzulenken. Alle Einstellungen sind mit wenigen Klicks erreichbar.

---

## Visuelle und funktionale Konsistenz

- UI-Elemente nutzen eine **einheitliche Farbpalette** (dunkle Hintergründe, leuchtende Highlights)
- Schaltflächen sind **klar abgegrenzt und animiert**
- Hover- und Klickzustände vermitteln **direktes Feedback**

**Designentscheidung:**  
Durch Pixel-Art und reduzierte Icons bleibt die Oberfläche optisch konsistent zur Fantasy-Welt, gleichzeitig modern genug für schnelle Navigation.

---

## Ausblick

Geplante Erweiterungen der Menü-UI:

- **Freundesliste & Einladungssystem**
- **Tooltips bei Maus-Hover**
- **Weitere Chat-Kanäle (z. B. Party, Support, Gilde)**
- **Fortschrittsanzeige für Achievements & tägliche Quests**


