---
title: YouTrack Templates
description: 
published: true
date: 2025-04-27T17:49:12.613Z
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

