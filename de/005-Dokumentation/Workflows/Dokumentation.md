---
title: Dokumentation
description: 
published: true
date: 2025-04-20T09:14:50.547Z
tags: 
editor: markdown
dateCreated: 2025-04-20T09:14:50.547Z
---

# ✅ Verifizierungsprozess – Kommentarbasierter Workflow mit Tagging

## 🎯 Ziel

Statt einer formalen Validierung im Wiki erfolgt die Abstimmung über **Kommentare unter den Seiten** und ein **verbindlich gepflegtes Tagging-System**.  
So werden Änderungen transparent gemacht, Diskussionen ermöglicht und der aktuelle Status von Inhalten ist für alle nachvollziehbar.

---

## 🔹 Initiale Erstellung & Sprint 0

- Alle neuen Seiten werden **ohne Tags** erstellt.
- Am Ende von **Sprint 0** erfolgt eine Teamprüfung.
- Danach werden alle geprüften und abgestimmten Seiten mit dem Tag `verifiziert` versehen.

---

## 🔹 Tagging-System: Übersicht & Bedeutung

Tags werden **sichtbar oben auf der Seite** angegeben.  
Sie zeigen den aktuellen Status des Inhalts und ermöglichen eine gezielte Filterung innerhalb des Wikis.

| Tag                        | Verwendung                                                                                  | Abgrenzung                                                                 |
|---------------------------|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| `verifiziert`             | Inhalt ist abgestimmt, aktuell und verbindlich.                                             | Standard-Tag nach erfolgreicher Abstimmung oder abgeschlossenem Review.    |
| `in_bearbeitung`          | Inhalt wird aktiv überarbeitet, aber Änderungen sind noch **nicht final**.                 | Wird gesetzt, sobald mit der Überarbeitung begonnen wurde.                 |
| `diskussion_bedarf`       | Inhalt steht zur Diskussion – Änderungsvorschläge liegen vor, aber noch **keine Entscheidung**. | Unterscheidet sich von `in_bearbeitung`: Noch keine aktive Bearbeitung.    |
| `verifizierung_ausstehend`| Änderungen wurden durchgeführt und müssen **noch abgestimmt/verifiziert** werden.          | Nach Bearbeitung, vor Setzen von `verifiziert`.                            |
| `ueberarbeitung_noetig`   | Inhalt ist veraltet oder unvollständig – Überarbeitung notwendig, aber noch **nicht beauftragt**. | Nur hinweisend – es wurde noch nichts gestartet.                           |

---

## 🛠️ Tagging-Regeln & Pflegehinweise

- **Tags müssen von allen Teammitgliedern aktiv gepflegt werden.**
- Jede inhaltliche Änderung wird mit einem passenden **Tag und einem Kommentar** dokumentiert.
- Bei Statusänderungen wird der **Tag aktualisiert**.
- Die **Filterung nach Tags** hilft, relevante Seiten für Meetings, Reviews oder Aufgaben schnell zu finden.

---

## 🔄 Typischer Ablauf mit Tags

1. Neue Seite → *(kein Tag)*
2. Nach Sprint 0 → `verifiziert`
3. Änderungsbedarf erkannt → `diskussion_bedarf` oder `ueberarbeitung_noetig`
4. Entscheidung zur Änderung → `in_bearbeitung`
5. Überarbeitung abgeschlossen → `verifizierung_ausstehend`
6. Nach Review → `verifiziert`

---

## 🗨️ Kommentare als Diskussionsraum

- Änderungswünsche, Rückfragen oder Einwände werden **als Kommentare direkt unter der Seite** gepostet.
- Der Kommentarthread dient als **zentrale Stelle für inhaltliche Abstimmungen**.
- Bei offenen Fragen oder Unsicherheiten kann der Eskalationsprozess genutzt werden:

### 🔺 Eskalationsprozess

1. Eine zweite Person wird zur Diskussion hinzugezogen.
2. Bei weiterer Unsicherheit → Thema wird ins Teammeeting oder in den Projekt-Chat gebracht.
3. Ziel: Klare Entscheidung mit minimalem Aufwand – ohne unnötige Belastung des Teams.

---