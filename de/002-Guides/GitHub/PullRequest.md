---
title: Guide: Pull Requests
description: In diesem Dokument wird eine Anleitung für Pull Requests dokumentiert.
published: true
date: 2025-05-11T09:42:01.363Z
tags: github, pullrequest
editor: markdown
dateCreated: 2025-05-11T09:42:01.363Z
---

# Pull Requests

Pull Requests werden durchgeführt um Änderungen in Branches in zentrale Branches zusammenzuführen. Damit Pull Requests einheitlich durchgeführt werden und sichergestellt wird das sich daran gehalten wird  werden in diesem Dokument einheitliche Regeln festgelegt an die sich gehalten werden muss.



## Zweck der Pull Requests
Eine Einheitliches vorgehen in den Pull Requests mit einem einheitlichen Prozess garantiert während der Entwicklung für eine höhere Qualität des entwickelten Produktes. Alle Maßnahmen sollten vom Aufwand angemessen dem zu bringenden Qualität stehen. Alle Maßnahmen sollten während der Nutzung stets darauf geprüft werden, ob sie die gewünschte Wirkung erzielen oder nur unnötigen Mehraufwand generieren.


## Zeitpunkte für einen Pull Request
Ein Pull Request kann von einem Feature oder Import auch in einem Zwischenstand gestellt werden, hierfür muss dieser seperat als solches gekennnzeichnet werden. Bekannte Fehler und schwächen müssen im Pull Request eindeutig dokumentiert sein. Es darf kein Runtimefehler enthalten sein der nicht durch einen throw new .... () Call ausgelöst wird,
Es darf keine Compiler Errors geben und die Anwendung muss im Editor ausführbar sein. Bereits funktionierende Systeme dürfen nicht beinträchtigt sein. 

Abschluss Pull Request liegt dann vor wenn ein Feature vollumfänglich abgeschlossen ist, hier soll ebenfalls sichergestellt werden das keine Compiler Errors vorhanden sind und Runtime Fehler bewusst ausgelöst werden ( Throw new ... () ).
Bekannte fehler und Probleme sollen ebenfalls dokumentiert werden.


## Inhalt der Pull Request
Ein ausführliches template im .md Format liegt für beide "Zeitpunkte" vor und kann in Github genutzt werden. Die hier genannten Inhalte sollten ordnungsgemäßg ausgefüllt werden, sodass jede Person aus dem Team den Pull Request bearbeiten kann. Es sollte grundlegenden die Aufgabe zu youTrack verlinkt werden, ebenfalls soll kompakt zusammen gefasst werden welche Inhalte enthalten sind, welche dinge geändert wurden.
Für Finale Pull Reuqests müssen
 Die Aktzeptanzkritieren aus den bezogenen YouTrack UserStories müssen ebenfalls dokumentiert worden sein und im Pull Request dokumentiert sein.
 
 Ebenfalls sollte in jedem Fall eine Anleitung zur Nutzung vorliegen und es sollte eine Anleitung für Tests in Unity vorliegen, welche durch den Reviewer ausgeführt werden können.
 
 
## Review Pull Request
Als Reviewer wird zuerst der Pull Request durchgelesen und geprüft ob die Dokumentation des Pull Requests korrekt durchgeführt wurde. Falls hier unklarheiten bestehen wird der Pull Requester kontaktiert und über diese informiert. Dieser ist anschließend in der Verantwortung den Pull Request dementsprechend zu überarbeiten. Erst wenn dies geschehen ist kann der Reviewer weiter machen.

Über die Option Files Changed können die geänderten Dateien angeschaut werden. 
![fileschanged001.png](/guide/github/pullrequest/fileschanged001.png)
Um sich nur bestimmte Dateien anzeigen zu lassen, die für das Review wichtig sind kann über die Option File Filter ein Dropdown Filter angezeigt werden in dem der Filter für das Review angezeigt werden kann. 
![fileschanged002.png](/guide/github/pullrequest/fileschanged002.png)
Für die meisten Reviews wenn sogar nicht alle ist es primär wichtig auf die geänderten Scripts zu achten. Hierfür wird alles abgewählt bis auf .cs Dateien. Bei Dateien wie .unity/ . prefab/ .asset sollte darauf geachtet werden das hier keine Hauptassets geändert wurden, es sei denn dies ist explizit im Pull Request geäußert und begründet, sodass dies eine Beabsichtige Entscheidung ist und nicht durch zufall, hierdurch werdne Merge Conflicts vermieden. In der Regel sollte an Zentralen Hauptassets nur nach Absprache bzw. Kommunikation im Team gearbeitet werden.
![fileschanged003.png](/guide/github/pullrequest/fileschanged003.png)
Während dem Review kann eine angeschaute Datei über die Toggle Box viewed eingeklappt werden, so sieht man als Reviewer wo man derzeit ist und hat eine Übersicht. Zusätzlich kann man allgemeine Anmerkungen und Fragen zu einer Datei als Kommentar hinzufügen.
![fileschanged004.png](/guide/github/pullrequest/fileschanged004.png)
Es ist ebenfalls möglich mit einem OnHover über eine Zeite ein Kommentar zu einer bestimmten Zeile hinzuzufügen. Hier öffnet sich ein Popup.
![fileschanged005.png](/guide/github/pullrequest/fileschanged005.png)
In diesem Fenster können Fragen oder Anmerkungen notiert werden. Ebenfalls kann über die Editor Buttons einige Funktionalität genutzt werden.
![fileschanged006.png](/guide/github/pullrequest/fileschanged006.png)
Als eine der wichtigsten bzw. praktischten ist die möglichkeit Änderungen vorzuschalgen, welche sich grundlegend erst mal auf eine Zeile beziehen, es ist aber möglich dies auf mehrere auzuweiten. Dies ermöglicht es dem Pull Requester im Pull Request den Vorschlag anzuschauen und direkt zu commiten ohne zusätzlich eine IDE öffnen zu müssen. So können Vorschläge direkt mitgeteilt werden ohne das diese von Reviewern direkt durchgeführt werden können. Es ist auch durchausmöglich und gewünscht das über die kommentarfunktion über den Vorschlag ausgetauscht wird. Dies ermöglicht einen Transparenten Arbeitsprozess der für alle Mitglieder nachvollziehbar ist. 
![fileschanged007.png](/guide/github/pullrequest/fileschanged007.png)
Ist das Review abgeschlossen kann über den blauen Button Review Changes alle Kommentare die gegeben worden sind dem Pull Request hinzugefügt werden. Erfolgt dies nicht sind diese Kommentare nur für einen selbst im Files Changed sichtbar. Dieser schritt ist essentiell und nötig für den Review Prozess. Hier gibt es 3 Optionen für den Review Result. Wenn man Reviewed hat aber keine endgültige Entscheidung treffen möchte kann man seine Änderungen als Kommentar hinzufügen. Sind alle Aspekte in Ordnung wird hier Approve ausgewählt. Hat man das gefühl das Dinge überarbeitet werden müssen oder vorschläge in betracht gezogen werden müssen kann hier über Request Change dies kommuniziert werden.
![fileschanged008.png](/guide/github/pullrequest/fileschanged008.png)

## Merge
Sind genug Approvals vorhanden und keine offenen Änderungsanfragen, kann der Pull Requester selbst aber auch der letzte Approver die Änderungen Mergen. Dies soll es ermöglichen das abgenommene Änderungen möglichstg schnell in den Hauptbranch gelangen.


