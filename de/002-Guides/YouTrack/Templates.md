---
title: YouTrack Templates
description: 
published: true
date: 2025-04-27T18:03:29.003Z
tags: verifizierung_ausstehend
editor: markdown
dateCreated: 2025-04-27T17:48:41.865Z
---

# User Story Template

## Titel
_Kurze, prägnante Zusammenfassung der Story_

## Beschreibung
Als **[Rolle]**
möchte ich **[Funktionalität]**,
um **[Nutzen]**.

## Akzeptanzkriterien
- [ ] [Kriterium 1: z.B. "Das Feature ist verfügbar, wenn Bedingung X eintritt."]
- [ ] [Kriterium 2: z.B. "Der Spieler erhält sichtbare Rückmeldung Y bei Aktion Z."]
- [ ] [Kriterium 3: z.B. "Fehlerfälle werden korrekt behandelt."]
- [ ] [Optional: Performance/UI Kriterien, z.B. "Ladezeit unter 2 Sekunden."]

## Zusatzinfos (optional)
- [Technische Hinweise, Assets, Mockups etc.]



# Task Template

## Task Name
_Kurzer, präziser Titel der Aufgabe_

## Beschreibung
- **Was ist zu tun?**
  _Kurze Erklärung der Aufgabe_
- **Warum ist es wichtig?**
  _Motivation oder Zusammenhang zur User Story_

## Akzeptanzkriterien (Definition of Done)
- [ ] Aufgabe ist vollständig implementiert
- [ ] Lokal erfolgreich getestet
- [ ] Pull Request erstellt und reviewt
- [ ] Dokumentation aktualisiert (Wiki, Code-Kommentare)
- [ ] Task ist mit zugehöriger User Story verlinkt


## Abhängigkeiten
_Andere Tasks oder Features, die zuerst abgeschlossen sein müssen_

## Anhänge/Links
_Mockups, technische Spezifikationen, zugehöriges Interface, Beispielcode etc._


---
# Checklisten
# User Story Vollständigkeits-Checkliste

Für jeden Punkt prüfen: **Erfüllt**, **Nicht notwendig**, oder **Offen/Klärung nötig**.

- [ ] **Rolle eindeutig definiert** (z.B. Spieler, Admin, NPC)
- [ ] **Ziel oder Funktionalität klar beschrieben** (Was genau soll erreicht werden?)
- [ ] **Nutzen oder Mehrwert erklärt** (Warum ist es relevant?)
- [ ] **Akzeptanzkriterien** vorhanden:
  - [ ] Erfolgsfall definiert
  - [ ] Fehler- oder Sonderfälle bedacht
  - [ ] Sichtbare Rückmeldungen spezifiziert (z.B. UI, Animationen, Sounds)

- [ ] **Visuelle oder auditive Elemente** erforderlich?
  - [ ] Falls ja, sind diese dokumentiert oder verlinkt?

- [ ] **Technische Anforderungen oder Einschränkungen** relevant?
  - [ ] Falls ja, dokumentiert?

- [ ] **Verbindungen zu anderen Systemen oder Modulen** notwendig?
  - [ ] Bezieht die Story Daten von anderen Systemen?
  - [ ] Liefert die Story Daten an andere Systeme?

- [ ] **Abhängigkeiten zu anderen Features oder Storys**?
  - [ ] Falls ja, dokumentiert?

- [ ] **Zusätzliche Beispiele, Flows oder Skizzen** ergänzt (falls hilfreich)?

---

**Hinweis:**  
> Wenn ein Punkt **nicht notwendig** ist, sollte dies ausdrücklich vermerkt werden (z.B. "Keine visuellen Elemente benötigt", "Kein Datenaustausch mit anderen Systemen").  
> Damit ist sichergestellt, dass bewusst geprüft und entschieden wurde.


# Task Vollständigkeits-Checkliste

Für jeden Punkt prüfen: **Erfüllt**, **Nicht notwendig**, oder **Offen/Klärung nötig**.

- [ ] **Titel beschreibt präzise die Aufgabe**

- [ ] **Beschreibung** enthält:
  - [ ] Was genau zu tun ist
  - [ ] Warum die Aufgabe notwendig ist

- [ ] **Erwartetes Ergebnis (Definition of Done)** klar formuliert

- [ ] **Assets (visuelle, auditive oder andere)** erforderlich?
  - [ ] Falls ja, sind sie verlinkt oder beschrieben?

- [ ] **Schnittstellen oder Abhängigkeiten zu anderen Systemen** notwendig?
  - [ ] Muss der Task Daten empfangen?
  - [ ] Muss der Task Daten liefern?

- [ ] **Fehler- und Sonderfälle** berücksichtigt (z.B. leere Antworten, ungültige Daten)?

- [ ] **Testhinweise** dokumentiert (Wie wird überprüft, ob der Task korrekt erledigt ist?)

- [ ] **Verlinkung zur zugehörigen User Story oder anderem Kontext** vorhanden?

- [ ] **Anhänge, Mockups oder Dokumentationen** ergänzt (falls erforderlich)

---

**Hinweis:**  
> Auch hier gilt: Wenn ein Punkt **nicht zutrifft**, sollte dies explizit festgehalten werden ("Keine externen Systeme involviert", "Keine Sonderfälle notwendig").



---

# User Story Beispiel
Inventarsystem – Gegenstand aufnehmen

## Beschreibung
Als **Spieler**
möchte ich **einen Gegenstand im Spiel aufheben und in mein Inventar legen**,
um **ihn später verwenden oder handeln zu können**.

## Akzeptanzkriterien
- [ ] Spieler können Gegenstände mit einem Klick/Knopfdruck aufnehmen.
- [ ] Aufgenommene Gegenstände erscheinen korrekt im Inventar-Slot.
- [ ] Das Inventar aktualisiert sich in Echtzeit.
- [ ] Fehlermeldung "Inventar voll!", wenn kein Platz mehr verfügbar ist.

## Zusatzinfos
- Asset-ID der Gegenstände: siehe /assets/items.md
- Inventar-UI Mockup: siehe Anhang


# Beispiel Task
Backend: Gegenstand aufnehmen API entwickeln

## Beschreibung
- API-Endpunkt erstellen, um "Gegenstand aufnehmen" zu unterstützen.
- Übergibt Spieler-ID und Item-ID.
- Validiert Inventarkapazität.
- Gibt Erfolg oder Fehlermeldung zurück.

## Akzeptanzkriterien (Definition of Done)
- [ ] POST `/api/pickup-item` implementiert
- [ ] Unit-Tests für Erfolgs- und Fehlerfälle vorhanden
- [ ] Pull Request erstellt und reviewed


## Abhängigkeiten
Abschluss des Inventar-Datenmodells

## Anhänge/Links
- [Inventar-Datenmodell Dokument](https://...)
- [API-Spezifikationsdokument](https://...)

