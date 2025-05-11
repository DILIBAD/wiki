---
title: Guide: Pull Requests
description: In diesem Dokument wird eine Anleitung für Pull Requests dokumentiert.
published: true
date: 2025-05-11T09:51:49.795Z
tags: github, pullrequest
editor: markdown
dateCreated: 2025-05-11T09:42:01.363Z
---

# Pull Requests

Pull Requests dienen dazu, Änderungen aus einem Branch in den zentralen Branch zu übernehmen. Um einheitliche Abläufe und eine hohe Qualität sicherzustellen, gelten folgende Regeln:

## 1. Zweck

Ein standardisierter Pull-Request-Prozess gewährleistet:

- **Qualitätssicherung:** Frühzeitiges Aufdecken von Fehlern und Schwächen.  
- **Effizienz:** Maßnahmen sollten im Verhältnis zum erwarteten Mehrwert stehen.  
- **Nachhaltigkeit:** Regelmäßige Überprüfung, ob etablierte Maßnahmen noch sinnvoll sind.

## 2. Zeitpunkte für einen Pull Request

### Zwischenstände  
- Kennzeichnung als „Zwischenstand“.  
- Bekannte Fehler und Schwächen im Pull Request dokumentieren.  
- Kein nicht abgefangener Runtime-Error (Ausnahmen nur via `throw new …`).  
- Kompilierungsfrei und ausführbar im Editor.  
- Bestehende Funktionalität darf nicht beeinträchtigt werden.

### Abschluss  
- Feature vollständig implementiert.  
- Keine Compiler-Fehler.  
- Laufzeitfehler nur bewusst ausgelöst (`throw new …`).  
- Bekannte Bugs und offene Punkte dokumentieren.

## 3. Inhalt

Nutze das vorgegebene Markdown-Template in GitHub und fülle es vollständig aus. Achte darauf:

- **YouTrack-Verknüpfung:** Verlinke die zugehörige Aufgabe.  
- **Kurzfassung:** Beschreibe, was geändert wurde.  
- **Akzeptanzkriterien:** Dokumentiere die aus der User Story.  
- **Anleitungen:**  
  - **Nutzung:** Kurzanleitung für die neue Funktion.  
  - **Tests in Unity:** Schritt-für-Schritt, damit Reviewer sie leicht nachvollziehen können.

## 4. Review

1. **Dokumentation prüfen:** Sind alle Template-Felder korrekt ausgefüllt?  
2. **Rückfragen klären:** Gegebenenfalls Pull-Requester kontaktieren.  
3. **Dateien ansehen:**  
   - Über **Files Changed** alle Änderungen einsehen.  
     ![fileschanged001.png](/guide/github/pullrequest/fileschanged001.png)  
   - Mit **File Filter** nur relevante Dateitypen, z. B. `.cs`.  
     ![fileschanged002.png](/guide/github/pullrequest/fileschanged002.png)  
   - Bei `.unity`, `.prefab`, `.asset`: Nur Hauptassets nach Absprache ändern, um Merge-Konflikte zu vermeiden.  
     ![fileschanged003.png](/guide/github/pullrequest/fileschanged003.png)  
4. **Kommentare und Vorschläge:**  
   - **Toggle View:** Überblick behalten und Dateien einklappen.  
     ![fileschanged004.png](/guide/github/pullrequest/fileschanged004.png)  
   - **Inline-Kommentare:** Per Hover an jeder Zeile.  
     ![fileschanged005.png](/guide/github/pullrequest/fileschanged005.png)  
   - **Editor-Buttons:** Zusätzliche Funktionen, z. B. Formatierung.  
     ![fileschanged006.png](/guide/github/pullrequest/fileschanged006.png)  
   - **Änderungsvorschläge:** Direkt im Editor erstellen, vom Pull-Requester übernehmbar.  
     ![fileschanged007.png](/guide/github/pullrequest/fileschanged007.png)  
5. **Review abschließen:**  
   - **Comment:** Nur Anmerkungen.  
   - **Approve:** Alles in Ordnung.  
   - **Request Changes:** Änderungen notwendig.  
   - **Text:** Eine Zusammenfassung aller Findings oder Protokoll der Tests die durchgeführt wurden
     ![fileschanged008.png](/guide/github/pullrequest/fileschanged008.png)

## 5. Merge

Sobald alle Review-Anfragen geklärt und genügend Approvals vorliegen, darf der Pull-Requester oder der letzte Approver die Änderungen in den Hauptbranch mergen. Dadurch gelangen bestätigte Änderungen schnell ins Zielsystem.


# Templates
<!-- TEMPLATE: Zwischenstand Pull Request -->
```
# Pull Request: Zwischenstand 

> _Kennzeichnung: Zwischenstand_NAME

---

## YouTrack-Verknüpfung
- **Ticket:** [TASK-XXXX](https://youtrack.example.com/issue/TASK-XXXX)

## Kurze Beschreibung
Beschreibe hier knapp den aktuellen Stand und die Zielsetzung dieses Zwischenstands.

## Änderungen
- Feature/Import:  
  - `Pfad/zu/Datei1.cs`: Kurzbeschreibung der Anpassung  
  - `Pfad/zu/Datei2.prefab`: Kurzbeschreibung der Anpassung  

## Dokumentierte bekannte Fehler & Schwächen
| Fehler/Beschreibung                   | Status      | Anmerkung                     |
|---------------------------------------|-------------|-------------------------------|
| Beispiel-NullReferenceException       | bekannt     | Tritt auf, wenn X nicht gesetzt |
| UI-Layout in Szene Y noch unvollständig | bekannt  | Muss in späterem Schritt angepasst werden |

## Technischer Zustand
- ❌ Keine nicht abgefangenen Runtime-Errors (Ausnahmen nur via `throw new …`)  
- ✅ Kompilierungsfrei und im Editor ausführbar  
- ✅ Bestehende Funktionalität unberührt  

## Nächste Schritte
1. Bekannte Bugs nach Release-X fixen  
2. UI-Feinschliff in Szene Y  
3. Abschluss-PR nach Fertigstellung des Features  
```
---

<!-- TEMPLATE: Abschluss Pull Request -->
```
# Pull Request: Abschluss

> _Kennzeichnung: Finaler Stand_NAME

---

## YouTrack-Verknüpfung
- **Ticket:** [TASK-XXXX](https://youtrack.example.com/issue/TASK-XXXX)

## Beschreibung
Ausführliche Zusammenfassung des vollständig abgeschlossenen Features.

## Änderungen
- `Pfad/zu/DateiA.cs`: Beschreibung der neuen Logik  
- `Pfad/zu/SzeneZ.unity`: Integration des Features  
- `Pfad/zu/AssetB.prefab`: Hinzugefügt für neue Funktionalität  

## Akzeptanzkriterien (aus User Story)
- [ ] Kriterium 1 erfüllt  
- [ ] Kriterium 2 erfüllt  
- [ ] Kriterium 3 erfüllt  

## Nutzung / Quickstart
1. Öffne Unity-Szene: `Pfad/zu/SzeneZ.unity`  
2. Drücke Play  
3. Gehe zu Menü → FeatureX  
4. Teste Aktion Y

## Tests in Unity
1. Projekt öffnen und kompilieren  
2. Szene `SzeneZ.unity` laden  
3. Im **Test Runner** folgende Tests ausführen:  
   - `TestSuite.FeatureXTests`  
   - `TestSuite.IntegrationTests`

## Technischer Zustand
- ✅ Keine Compiler-Fehler  
- ✅ Laufzeitfehler nur bewusst ausgelöst (`throw new …`)  
- ❌ Keine ungeklärten Bugs (alle offenen Punkte behoben oder dokumentiert)


```
---
