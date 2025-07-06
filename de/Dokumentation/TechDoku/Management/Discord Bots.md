---
title: Modulmatrix
description: 
published: true
date: 2025-07-05T14:33:57.270Z
tags: 
editor: markdown
dateCreated: 2025-04-28T14:41:29.883Z
---
# Discord-Bots

## Übersicht  
Für das Projekt DILIBAD wurden drei unterschiedliche Discord-Bots eingesetzt, um die interne Kommunikation, Fehlerverfolgung und CI/CD-Prozesse effizient zu unterstützen. Die Bots sind eng mit GitHub und weiteren Tools verknüpft und helfen dabei, das Entwicklerteam auch außerhalb der klassischen Tools wie GitHub oder U-Track über den aktuellen Projektstatus auf dem Laufenden zu halten.

---

## 1. Updates-Bot – „Daily Dev Summary“

### Zweck  
Der Updates-Bot aggregiert täglich die individuellen Entwickler-Updates aus einem dedizierten Channel. Jeder Entwickler postet am Ende des Tages seine Fortschritte. Der Bot fasst diese in einem übersichtlichen Format zusammen.

### Nutzen  
- Transparenz über tägliche Fortschritte  
- Übersicht auch für Teammitglieder, die nicht aktiv an jedem Feature beteiligt sind  
- Alternative Ergänzung zur reinen Git-Historie  
- Motivation durch tägliche Erfolge sichtbar machen  

### Beispielhafte Einträge  
- ✅ „Buff/Debuff hinzugefügt und Targeting-System überarbeitet“  
- ✅ „Loot Table Implemented“  
- ✅ „Interaction Prompts eingeführt“

➡️ Die Zusammenfassung wird automatisch in einem Channel wie `#daily-dev-summary` gepostet.

![[Pasted image 20250706180341.png]]

---

## 2. Steam-Build-Bot – „CI/CD Build Tracker“

### Zweck  
Dieser Bot ist Teil der Continuous Integration und Deployment Pipeline. Er gibt automatisch eine Rückmeldung auf Discord, wenn ein neuer Build auf Steam deployed wurde.

### Funktionen  
- Postet in einen Discord-Channel den aktuellen **Build-Status**  
- Informiert über:  
  - `build-remote-client success`  
  - `deploy-remote-client success`  
  - GitHub Actions Checks  
  - `publish-to-steam success`

### Nutzen  
- Jeder im Team weiß sofort, welche Version aktiv deployed ist  
- Kein Nachfragen im Team nötig  
- Rückmeldung in Echtzeit  

➡️ Ideal für QA und Playtests, um sicherzustellen, dass alle dieselbe Version spielen.

![[Pasted image 20250706180400.png]]

---

## 3. Bugtracker-Bot – „/bugreport Bot“

### Zweck  
Dieser Bot dient dazu, Bugs direkt im Discord zu erfassen und automatisiert als GitHub Issue anzulegen.

### Funktionsweise  
1. Der Nutzer gibt `/bugreport open` im Discord ein.  
2. Es öffnet sich ein Formular mit:  
   - Titel des Bugs  
   - Kurzbeschreibung  
   - Reproduktionsschritte (ein Schritt pro Zeile)  
3. Nach Absenden:  
   - Wird der Bug als GitHub Issue erstellt (inkl. Formatierung)  
   - Es erscheint im Discord ein Vorschau-Post mit Buttons:
     - ✅ Reproduzierbar  
     - ❌ Nicht reproduzierbar  
     - 💬 Kommentar  

### Vorteile  
- Reduziert Reibungsverluste zwischen Testern und Entwicklern  
- Kein technisches GitHub-Wissen nötig  
- Direkte Einbindung in Entwickler-Workflow  
- Community-Feedback über Buttons möglich  

### Beispiel  
> **Bug #5: Gegneranimation frieren ein**  
> „Wenn 3 Gegner gleichzeitig angreifen, friert einer ein…“

➡️ Verlinkung zwischen Discord und GitHub ist bidirektional. Entwickler können direkt im Issue weiterarbeiten.

![[Pasted image 20250706180415.png]]

![[Pasted image 20250706180420.png]]

![[Pasted image 20250706180439.png]]


---

## Fazit  
Die drei Discord-Bots waren ein integraler Bestandteil der agilen Zusammenarbeit im Projekt DILIBAD.  
Sie unterstützten sowohl die technische Seite (Builds, Bugs) als auch die menschliche Komponente (Transparenz, Kommunikation). Besonders in einem studentischen Umfeld mit verteilten Arbeitszeiten bieten solche Tools einen enormen Mehrwert für Effizienz und Übersicht.
