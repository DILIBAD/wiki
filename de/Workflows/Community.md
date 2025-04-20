---
title: Community
description: 
published: true
date: 2025-04-20T08:42:31.940Z
tags: 
editor: markdown
dateCreated: 2025-04-20T08:40:42.461Z
---

# Community

Als *Community* werden alle Personen bezeichnet, die nicht aktiv an der Entwicklung von **DILIBAD** beteiligt sind, jedoch den Fortschritt verfolgen, Testversionen spielen und Feedback oder Ideen einbringen.

## Ziel der Community

Der Aufbau einer Community verfolgt primär das Ziel, während der Entwicklungsphase externes Feedback zu erhalten. Darüber hinaus ermöglicht die Community den Aufbau einer umfassenden Teststruktur, die vom Entwicklungsteam allein nicht abgedeckt werden könnte. 

Viele Bugs entstehen durch spezifische Abfolgen von Aktionen, die Entwicklern oft nicht auffallen, da sie meist dem vorgesehenen Ablauf folgen. Durch vielfältige Tests seitens der Community lassen sich solche Fehler früher erkennen, was letztlich zu einem qualitativ hochwertigeren Endprodukt führt.

## Disclaimer

Auch wenn der Community-Aufbau ein wichtiges Teilziel ist, soll der dafür aufgewendete Zeit- und Ressourcenaufwand **nicht mehr als 10 %** der gesamten Entwicklungskapazitäten überschreiten. Der Fokus liegt weiterhin auf der Spielentwicklung.

## Plattformen

Zur Kommunikation mit der Community werden verschiedene Social-Media-Kanäle wie Instagram, X (ehemals Twitter), Threads und ähnliche Plattformen genutzt, um die gewünschte Zielgruppe zu erreichen.

Alle Inhalte und Informationen laufen zentral auf der offiziellen Webseite [https://dilibad.de](https://dilibad.de) zusammen. Diese wird initial mithilfe von [Loveable](https://lovable.dev/) über Prompts generiert, wodurch ein React-basiertes Frontend entsteht. Die Seite ist mit GitHub verbunden und kann zusätzlich manuell bearbeitet werden.

Um die DSGVO-Konformität zu gewährleisten, werden die Inhalte über ein Headless CMS (z. B. **Strapi**) auf einem dedizierten Server in Deutschland gehostet. Ein statisches Content Delivery System sorgt dafür, dass Inhalte effizient ausgespielt werden können. Die Pflege erfolgt bequem über das CMS-Backend und reduziert so den Programmieraufwand während der Entwicklungszeit.

Ein **Roadmap-Bereich** auf der Webseite wird idealerweise automatisiert über eine API mit Aufgaben aus **YouTrack** gefüllt, sodass keine manuelle Pflege notwendig ist.

### Discord-Integration

Zur direkten Kommunikation mit der Community wird ein **Discord-Server** betrieben. Dort unterstützt ein Bot mit Befehlen wie:

- `/idea "Idee"`  
- `/bug "Bugbeschreibung"`  
- `/feedback "Feedback"`

Diese Eingaben werden automatisch in GitHub-Diskussionen oder Issues überführt. Alternativ können Nutzer ihre Beiträge direkt über GitHub einreichen.

So wird sichergestellt, dass Feedback zentral gesammelt wird, ohne dass ein Teammitglied dies aktiv übernehmen muss. Das Team prüft regelmäßig die eingegangenen Beiträge und priorisiert sie. Dabei wird unterschieden zwischen:

- kurzfristig relevanten Inhalten (z. B. Bugs)  
- langfristigen Vorschlägen (z. B. neue Features, Konzeptideen)

### Testversionen

Mögliche Plattformen für testbare Versionen sind derzeit:

- Web-Version  
- [itch.io](https://itch.io)  
- Steam

Details hierzu werden in einem separaten Workflow-Dokument festgehalten.

## Risiken und Vorsichtsmaßnahmen

Die Integration einer aktiven Community bringt viele Vorteile, aber auch Risiken mit sich. Diese sollten klar benannt und im Projektverlauf regelmäßig evaluiert werden. Zu den wichtigsten Risiken zählen:

- **Burnout im Team**: Durch zu hohen Input, Druck oder unerwartete Anforderungen seitens der Community kann es zu Überlastung kommen. 
- **Scope Creep**: Zu viele Feature-Wünsche oder unstrukturierte Anforderungen können das Projekt vom ursprünglichen Kurs abbringen und die Entwicklung erheblich verzögern.
- **Reputationsrisiken**: Unzufriedene oder toxische Community-Mitglieder können negative Publicity erzeugen, insbesondere bei fehlendem Community-Management.
- **Technischer Overhead**: Der Betrieb von Plattformen wie CMS, Discord-Bots, API-Anbindungen etc. kann zu zusätzlichem Wartungsaufwand führen.
- **Ressourcenumlenkung**: Zu viel Zeit in der Moderation oder technischen Pflege kann vom eigentlichen Entwicklungsziel ablenken.
- **Fehlgeleitetes Feedback**: Nicht alle Vorschläge aus der Community sind zielführend oder qualitativ hochwertig – Filtermechanismen sind erforderlich.

### Exit- / Rückzugstrategie

Sollte sich im Projektverlauf herausstellen, dass die Community-Aktivitäten zu großen Schaden am Entwicklungsprozess verursachen (z. B. durch Überforderung, Burnout, Verzögerung oder interne Konflikte), tritt folgende Rückzugsstrategie in Kraft:

1. **Interne Eskalationsmeldung**  
   Jedes Teammitglied kann potenzielle Überlastung oder Community-bezogene Konflikte anonym oder direkt melden.

2. **Evaluationsphase (max. 1 Woche)**  
   Das Kernteam analysiert in dieser Zeit:
   - Belastungsstand des Teams
   - Verhältnis zwischen Entwicklungsfortschritt und Community-Aufwand
   - Nutzen der bisherigen Community-Beiträge

3. **Sofortmaßnahmen zur Entlastung**
   - Pausierung oder Reduktion der Interaktionen auf Discord (z. B. Abschalten der Bot-Funktionen)
   - Social-Media-Kommunikation wird auf ein Minimum reduziert
   - Eingehendes Feedback wird gesammelt, aber nicht priorisiert oder bearbeitet

4. **Kommunikation nach außen**  
   Die Community wird transparent über eine geplante Umbau- oder Pausenphase informiert. Ziel ist es, Verständnis und Vertrauen aufrechtzuerhalten.

5. **Schrittweise Reaktivierung (optional)**  
   Sollte sich die Belastungssituation entspannen, kann der Community-Prozess schrittweise reaktiviert werden, z. B. durch:
   - Wiedereinführung einzelner Kanäle (Roadmap, Feedback)
   - Fokus auf gezielte Feedbackphasen (z. B. Testwochen vor Releases)

6. **Alternativ: Dauerhafte Reduktion**  
   Falls eine Reaktivierung nicht sinnvoll oder machbar ist, wird die Community dauerhaft auf passiven Modus umgestellt (Informationsplattform statt Interaktionsplattform).

**Wichtig:** Die Gesundheit und Motivation des Teams haben stets Vorrang. Die Community ist ein wertvoller Bestandteil, aber kein Selbstzweck.



## Fazit

Der frühzeitige Aufbau einer Community soll helfen, durch den kollektiven Beitrag vieler (*"Hive Mind"*) ein besseres Spiel zu entwickeln. Der beschriebene Workflow stellt den Idealfall dar und dient als Leitlinie. 

Dabei ist stets zu beachten: **Der Aufwand muss im Verhältnis zum Nutzen stehen.** Der Hauptfokus liegt weiterhin auf der Entwicklung. 

Eine kontinuierliche Überprüfung dieses Prozesses ist erforderlich, um sicherzustellen, dass er den Gesamtumfang nicht sprengt und keine Überlastung im Team verursacht. Sollte sich ein Teammitglied überlastet fühlen – bitte offen im Team kommunizieren!
