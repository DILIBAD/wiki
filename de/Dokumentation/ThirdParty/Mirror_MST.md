---
title: ThirdParty
description: 
published: true
date: 2025-07-05T15:40:08.613Z
tags: 
editor: markdown
dateCreated: 2025-07-05T15:40:08.613Z
---

# ## Entscheidung für Frameworks: Mirror & Master Server Toolkit (MST)

Für das Projekt DILIBAD wurde die Entscheidung getroffen, Mirror als primäre Netzwerkbibliothek und das Master Server Toolkit (MST) zur Ergänzung der Serverfunktionalitäten zu nutzen. Diese Wahl basiert auf den Anforderungen aus dem Game Design Document (GDD), insbesondere den kooperativen Multiplayer-Mechaniken, der Flexibilität bei der Spielerzahl sowie dem Bedarf an dedizierten Serverlösungen.

### Mirror

**Kurzbeschreibung:** Mirror ist ein Open-Source-Netzwerk-Framework für Unity, das die einfache Erstellung von Multiplayer-Spielen ermöglicht. Mirror bietet eine robuste und effiziente Synchronisation von Spielzuständen zwischen Server und Clients.

**Funktionsweise:** Mirror arbeitet mit einem Client-Server-Modell, bei dem ein Server den Spielzustand verwaltet und mit allen verbundenen Clients synchronisiert. Änderungen am Spielzustand werden automatisch über Netzwerkkomponenten (wie z.B. NetworkBehaviour, SyncVars und RPCs) abgeglichen.

**Vorteile für DILIBAD:**

- Einfache und zuverlässige Synchronisation der Spielmechaniken (Positionen, Gegnerzustände, Ressourcenverwaltung).
    
- Unterstützung für dedizierte Server und Peer-to-Peer-Lösungen.
    
- Effiziente Integration in die Unity-Engine, was Entwicklungszeit spart.
    
- Open Source mit aktiver Community, was langfristigen Support sichert.
    

### Master Server Toolkit (MST)

**Kurzbeschreibung:** Das Master Server Toolkit ergänzt Mirror um eine erweiterte Server-Infrastruktur, welche insbesondere Matchmaking, Authentifizierung, Account-Verwaltung und dedizierte Serververwaltung beinhaltet.

**Funktionsweise:** MST stellt zentrale Services wie Authentifizierung, Benutzerverwaltung und Serverregistrierung bereit. Es ermöglicht das Erstellen von Matchmaking-Systemen, die Verbindung zu dedizierten Spielservern sowie das Management von Spieleraccounts und Lobbys.

**Vorteile für DILIBAD:**

- Zentralisierte Verwaltung von dedizierten Servern und Spielerlobbys.
    
- Unterstützung von sicheren Authentifizierungsmechanismen.
    
- Erweiterte Skalierbarkeit und Flexibilität für Multiplayer-Matches.
    
- Optimal für das geplante Session-basierte Gameplay und die Verwaltung größerer Spielerzahlen geeignet.
    

### Fazit

Die Kombination aus Mirror und MST erfüllt optimal die im GDD definierten Anforderungen:

- Robuste Synchronisation im kooperativen Gameplay (Ressourcenabbau, Basisbau, Gegnerwellen, Bosskämpfe).
    
- Dedizierte Serveroption für größere, langfristige Multiplayer-Sessions (Living World).
    
- Sichere und flexible Verwaltung von Multiplayer-Session und Spieleraccounts.
    
- Skalierung durch mehreren Mirror Instanzen von 4 Spielern auf 1000 Spieler
    
Damit sind diese Frameworks essenziell für die Umsetzung des kooperativen, skalierbaren und session-orientierten Multiplayer-Konzepts von DILIBAD.

