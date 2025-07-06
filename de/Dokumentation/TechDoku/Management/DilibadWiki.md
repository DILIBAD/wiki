---
title: Modulmatrix
description: 
published: true
date: 2025-07-05T14:33:57.270Z
tags: 
editor: markdown
dateCreated: 2025-04-28T14:41:29.883Z
---

# Wiki-Integration

## TL;DR  
Das offizielle DILIBAD-Wiki ist über [wiki.dilibad.de](https://wiki.dilibad.de) erreichbar. Es ist direkt mit unserem Git-Repository verknüpft, sodass Änderungen automatisch synchronisiert werden. Zur Bearbeitung kann man sich bequem über Discord anmelden.

---

## Zweck / Ziel  
Ziel des Wikis ist es, eine zentralisierte, leicht zugängliche und kollaborative Dokumentationsplattform bereitzustellen.  
Es ermöglicht:

- **Einfache Bearbeitung durch Teammitglieder** – auch ohne tiefere Git-Kenntnisse  
- **Direkte Synchronisierung mit dem GitHub-Repository** – keine manuelle Übertragung notwendig  
- **Zugriffskontrolle via Discord** – nur berechtigte Nutzer:innen können Änderungen vornehmen  
- **Versionskontrolle und Historie** – jede Änderung ist rückverfolgbar  

---

## Umsetzung

- Das Wiki ist unter **https://wiki.dilibad.de** erreichbar.
- Technisch basiert es auf einer Git-Wiki-Integration mit automatischem Push zum Repository `github.com/dilibad/wiki`.
- Änderungen am Wiki werden entweder:
  - direkt über die Weboberfläche vorgenommen (mit Login über Discord), oder
  - lokal über Git gepusht.
- Änderungen in der Weboberfläche triggern automatisch einen Commit und Push ins Git-Repo.
- Jeder Eintrag im Wiki liegt als `.md`-Datei im Repo – dadurch auch offline bearbeitbar.

---

## Login & Zugriffssteuerung

- Die Authentifizierung erfolgt über **Discord OAuth**.
- Nur Discord-Mitglieder mit entsprechender Rolle (z. B. `Editor`, `Team`) können Beiträge verfassen oder ändern.
- Anonyme oder unangemeldete Änderungen sind **nicht** möglich.

---

## Reflektion

Die Kombination aus Markdown-Wiki, Git-Integration und Discord-Login bietet eine **sehr niedrige Einstiegshürde** und ermöglicht dennoch professionelle Dokumentationspflege.  
Es hat sich als effektives Werkzeug für unser gesamtes Team erwiesen – sowohl zur Live-Bearbeitung als auch für langfristige Archivierung im Git.


---

## Offene Punkte

- Einige Nutzer benötigen ggf. eine kurze Einführung in Markdown
- Berechtigungen müssen bei größeren Teamänderungen regelmäßig gepflegt werden
