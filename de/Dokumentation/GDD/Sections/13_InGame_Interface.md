# 4. In-Game Interface & Spielerinteraktion

## 4.1 Überblick

Die Benutzeroberfläche (UI) ist ein zentraler Bestandteil des Spielerlebnisses. Sie wurde so gestaltet, dass alle relevanten Informationen intuitiv auffindbar sind, ohne das Spielgeschehen zu überfrachten. Dabei wurde auf klare Strukturen, schnelle Rückmeldung und visuelle Hierarchie geachtet.

---

## 4.2 Ressourcenanzeige (oben rechts)

In der oberen rechten Ecke des Bildschirms werden die gesammelten **Ressourcen** angezeigt.
Diese umfassen:

- **blaue Kristalle**
- **rote Key Kristalle** 
- **grüne Lebens Kristalle** 

hierbei handelt es sich um die Ressourcen die im Gemeinschaftsinventar liegen

**Designentscheidung:**  
Die Position oben rechts wurde gewählt, um während des Spielens einen schnellen Blick auf alle Ressourcen zu ermöglichen – ohne den Fokus vom Spielfeld abzulenken und klar getrennt von dem Eigenen Inventar zu sein.

---

## 4.3 Spieleranzeige (unten mittig)

Unten in der Mitte befindet sich das **Spielerinterface**, bestehend aus:

- **HP-Anzeige (Lebenspunkte):** visuelle Darstellung der verbleibenden Gesundheit
- **Skillslots:** aktive Fähigkeiten des Spielers, mit Cooldown-Indikatoren

**Designentscheidung:**  
Das Interface wurde mittig platziert, um maximale Kontrolle und Übersicht im Gefecht zu ermöglichen. Die Cooldown-Anzeige ist farblich abgestuft, um Restzeiten schnell erfassbar zu machen.

---

## 4.4 Inventar & Wave-Anzeige (unten rechts)

Im rechten unteren Bereich werden zwei zentrale Elemente kombiniert:

- **Gefahrenlevel / Wave-Anzeige:** zeigt an, wie viele Gegnerwellen bereits besiegt wurden bzw. wie viele folgen
- **Spieler-Inventar:** visuelle Übersicht aller eingesammelten Gegenstände

**Designentscheidung:**  
Die Kombination aus beiden Elementen schafft Übersichtlichkeit. Das Wave-System wurde so integriert, dass es eine **steigende Spannungskurve** erzeugt.

---

## 4.5 Interaktive Objekte

### Upgrade-Tisch

Am **Upgrade-Table** kann der Spieler:

- **Statistiken** einsehen (z. B. Schaden, Verteidigung)
- **Passive Effekte** auswählen oder verbessern
- **Teamweite Boni** aktivieren (nur mit Ressourcen möglich)

### Crafting-Station

Die **Crafting-Station** dient der:

- Herstellung von **Turrets** und **Consumables**
- Verbesserung eigener **Skills** durch neue Module
- **Ressourcentransformation** (z. B. Erz zu Barren)

**Designentscheidung:**  
Crafting und Upgrades wurden voneinander getrennt, um klare Spielphasen zu etablieren: **Optimieren (Upgrades)** und **Vorbereiten (Crafting)**.

---

### Teleporter

Mit dem **Teleporter** kann der Spieler:

- Zwischen Karten wechseln
- Neue Gebiete erkunden

**Designentscheidung:**  
Der Teleporter eröffnet taktische Optionen und erlaubt, sich **zwischen Basis und Missionsgebiet** zu bewegen, ohne lange Laufwege.

---

## 4.6 Ausblick

Die UI ist modular aufgebaut und wird künftig um neue Anzeigen (z. B. Gruppenanzeige im Multiplayer) erweitert. Auch Tooltip-Systeme und kontextabhängige Hinweise sind in Planung.

