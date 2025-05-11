---
title: Guide: Pull Requests
description: In diesem Dokument wird eine Anleitung für Pull Requests dokumentiert.
published: true
date: 2025-05-11T09:45:10.902Z
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
