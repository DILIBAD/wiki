# Scriptable Objects und NPC-System in Unity

Für die Umsetzung der Gegner, Fähigkeiten, Zustände (States) und Werte (Stats) wurden in Unity ScriptableObjects verwendet. Ein ScriptableObject ist ein spezieller Objekttyp, der es ermöglicht, Daten und Logik unabhängig von Szenen oder GameObjects zu verwalten. Sie eignen sich ideal für zentrale, wiederverwendbare Datencontainer wie Konfigurationsdaten, KI-Zustände, Fähigkeiten oder Item-Definitionen. Diese Objekte werden im Projekt als .asset-Dateien gespeichert und sind nicht an bestimmte Instanzen gebunden.

Das StatsDataTemplate ist ein ScriptableObject, das als zentrale Vorlage zur Konfiguration von Basiswerten (Stats) für Einheiten im Spiel dient. Es implementiert das IStatsDataTemplate-Interface und definiert grundlegende Eigenschaften wie Bewegungsgeschwindigkeit, Angriff, Angriffsgeschwindigkeit, Verteidigung, Lebenspunkte, Regeneration, Reichweite und Sichtweite.

Diese Werte können im Unity-Editor editiert und als .asset gespeichert werden. Dadurch lassen sich einheitliche Stat-Vorlagen für verschiedene Einheitentypen (z. B. NPCs, Spieler, Tower) flexibel und wiederverwendbar konfigurieren.

Das ModifyStatsDataTemplate ist ein weiteres ScriptableObject, das temporäre Modifikationen von Stat-Werten (wie z. B. Buffs oder Debuffs) beschreibt. Es definiert, welcher Stat-Typ modifiziert wird, mit welchem Wert, für welche Dauer, und mit welchem Operator (z. B. Additiv oder Multiplikativ).

Das SkillDataTemplateBase ist ein zentrales ScriptableObject zur Definition von Fähigkeiten. Es enthält allgemeine Eigenschaften wie Cooldown, Reichweite, Animationseinstellungen, Effektdelay sowie ein Icon und Lokalisierungsschlüssel für Name und Beschreibung. Über optionale Nützlichkeitsprüfungen kann bewertet werden, ob eine Fähigkeit in der aktuellen Situation sinnvoll ist. Zudem definiert es eine Reihe von virtuellen Methoden, die bei verschiedenen Phasen der Fähigkeit (z. B. Start, Ausführung, Abschluss) sowohl client- als auch serverseitig ausgelöst werden können. Damit bildet es die Grundlage für konkrete Skill-Implementierungen im Spiel.

---

## NPC-Struktur und Fraktion

Im Spiel gibt es aktuell drei Fraktionen, die jeweils über unterschiedliche Gegner, Minibosse und Bosse verfügen. Die Gegner sind in sechs verschiedene Rangtypen unterteilt, die sich in ihrer Stärke unterscheiden:

- Minion
- Grunt
- Veteran
- Elite
- Nightmare
- Legendary

Diese Einteilung ermöglicht es, schnell eine Vielzahl unterschiedlich starker Gegner zu definieren. Jeder NPC besitzt individuelle Werte (Stats), einen eigenen Animator Controller, sowie eine Auswahl an Fähigkeiten (Skills) und KI-Zuständen (States).

---

## State-System und NpcStateMachine

Die Steuerung des NPC-Verhaltens erfolgt über eine NpcStateMachine, die auf einer Prioritätslogik basiert. Diese verwaltet eine Liste möglicher Zustände und aktiviert automatisch den Zustand mit der höchsten Bewertung, sofern dieser sich vom aktuellen unterscheidet. Jeder Zustand (State) implementiert die Methoden Enter, Execute, Exit und Evaluate.

**Beschreibung der wichtigsten Zustände**

- **IdleNpcState**: Der passive Zustand. Wird aktiv, wenn kein Bewegungs- oder Kampfziel vorhanden ist. Der NPC bleibt stehen, während sich ein "Boredom"-Wert kontinuierlich erhöht.
- **WanderingNpcState**: Wird aktiviert, wenn keine Kampfhandlung vorliegt und der Boredom-Wert einen Schwellenwert überschreitet. Der NPC wählt ein zufälliges Ziel im Umkreis und bewegt sich dorthin. Das eigentliche Bewegen übernimmt der MovementNpcState.
- **MovementNpcState**: Steuert die gezielte Fortbewegung des NPCs zu einem bestimmten Ziel mit dynamischer Stopping-Distance. Nur aktiv, wenn ein Ziel existiert, nicht erreicht ist und kein Skill verwendet wird.
- **AttackNpcState**: Verantwortlich für das Kampfverhalten. Prüft regelmäßig, ob ein Angriff möglich ist, richtet den NPC korrekt aus und führt eine verfügbare Fähigkeit aus.
- **PickSkillState**: Dieser Zustand sorgt dafür, dass der NPC im Kampf regelmäßig eine einsetzbare Fähigkeit auswählt. Wird aktiv, wenn keine Fähigkeit vorhanden ist oder eine vorherige Fähigkeit über einen gewissen Zeitraum hinweg nicht verwendet wurde.
- **EnrageState (für Bosse)**: Wird ausgelöst, wenn ein Ziel entdeckt wurde und eine bestimmte Zeit vergangen ist. Der NPC ignoriert andere Gegner, bewegt sich zum Mittelpunkt der Arena und führt dort eine Spezialfähigkeit aus. Nach Ablauf eines Timers wird sein Leben vollständig regeneriert, und der Zustand endet.
- **InvulnState**: Versetzt Bosse temporär in einen unverwundbaren Zustand. Beim Eintritt wird der NPC immun gegen Schaden und startet eine World Action am Zielpunkt. Nach Ablauf eines Timers wird die Unverwundbarkeit wieder aufgehoben, und der NPC verlässt diesen Zustand.

---

## Kontextsystem und Datenkommunikation

Ein zentrales Element zur Kommunikation zwischen Zuständen und Komponenten ist die ProvideContext-Komponente, welche ein Blackboard-System nutzt. In diesem Memory-System können Daten zentral gespeichert und abgerufen werden, ohne direkte Kopplung zwischen Komponenten oder Zuständen. So können etwa Bewegungsziele, Timer oder Zustandsflags effizient ausgetauscht werden.

Die Komponente liest relevante Informationen aus dem Memory aus und kombiniert sie zu logischen Kontexten, die den Zuständen zur Verfügung stehen. Durch dieses System ist das NPC-System modular, flexibel erweiterbar und gut wartbar, da Zustände und Komponenten unabhängig voneinander agieren können.

---

## Skilltypen und Arten

Im Rahmen des aktuellen Projekts werden verschiedene Skilltypen implementiert, die unterschiedliche Funktionsweisen und Anwendungsbereiche innerhalb des Spiels abdecken.

### Projektil-Skills

## Bestandteile und Ablauf des Projektil-Skills

1. **Skill-Aktivierung**
    
    - Der Spieler aktiviert den Skill.
    - Serverseitig wird geprüft, ob der Skill einsatzbereit ist.
    - Nach einer optionalen Verzögerung wird der eigentliche Effekt (Projektil) ausgelöst.
2. **Projektil-Erzeugung**
    
    - Ein Projektil wird an einer leicht erhöhten Position vor dem Anwender gespawnt.
    - Es richtet sich nach der Blickrichtung des Anwenders.
    - Das Projektil erhält seine Konfigurationsdaten und wird aktiviert.
3. **Flug und Treffererkennung**
    
    - Das Projektil bewegt sich mit konfigurierter Geschwindigkeit in gerader Linie.
    - Kollisionen mit Gegnern werden überprüft.
    - Es kann einzelne oder mehrere Ziele gleichzeitig bzw. hintereinander treffen.
4. **Schadensermittlung**
    
    - Bei einem Treffer wird geprüft, ob das Ziel Schaden empfangen kann.
    - Der Schaden berechnet sich aus Projektildaten und Attributen des Anwenders.
    - Wird eine maximale Anzahl an Treffern oder die Lebensdauer erreicht, wird das Projektil entfernt.

## ⚙️ Einstellungsoptionen des Projektil-Skills

### 1. Projektil-Konfiguration

- **Prefab:** Visuelles und logisches Objekt, das instanziiert wird.
- **Schaden:** Basis-Schaden des Projektils (vor Modifikatoren).
- **Geschwindigkeit:** Bewegungsgeschwindigkeit pro Sekunde.
- **Lebensdauer:** Wie lange das Projektil existiert, bevor es verschwindet.
- **Radius:** Wirkungsbereich bei Flächenangriffen.
- **Maximale Ziele:** Wie viele Einheiten gleichzeitig getroffen werden dürfen.
- **Durchdringung:** Wie viele Ziele das Projektil nacheinander durchdringen darf.
- **Layer-Maske:** Welche Objekte als gültige Ziele betrachtet werden.

### 2. Zusatzkonfiguration

- **Höhenkorrektur beim Spawnen:** Positionierung des Projektils leicht oberhalb der Einheit (z. B. bei 2D-Topdown-Spielen relevant für optische Klarheit).

### Turret- und Deployable-Skills

## Bestandteile und Ablauf des Turret-Placement-Skills

1. **Skill-Aktivierung und Vorschau**
    
    - Der Spieler startet die Skill-Aktivierung (z.B. Button gedrückt).
    - Für Spieler wird eine Vorschau des Turms an der möglichen Platzierungsposition angezeigt.
    - Die Vorschau zeigt den erlaubten Platzierungsbereich basierend auf der Skill-Reichweite.
    - Beim Loslassen des Skill-Buttons wird die Vorschau wieder entfernt.
2. **Turmplatzierung**
    
    - Serverseitig wird geprüft, ob ein Turm-Template zugewiesen ist.
    - Der Spieler fordert das Platzieren des Turms an.
    - Der Turm wird an der vom Spieler gewählten Position erzeugt und ins Spiel eingebunden.

---

## ⚙️ Einstellungsoptionen des Turret-Placement-Skills

- **Maximale Anzahl Türme:** Wie viele Türme ein Spieler gleichzeitig platzieren darf.
- **Reichweite:** Maximale Distanz vom Spieler, innerhalb der der Turm platziert werden kann.
- **Turm-Template:** Referenz auf das Turm-Objekt, das instanziiert wird.
- **Vorschau-Prefab:** Visuelle Darstellung zur Platzierungsvorschau vor endgültiger Platzierung.

### Nahkampf-Skills

## 🧩 Bestandteile und Ablauf des Nahkampf-Skills

1. **Skill-Aktivierung**
    
    - Der Spieler aktiviert den Nahkampf-Skill.
    - Wenn ein Indikator-Objekt definiert ist, wird dieses an der Position des Anwenders erzeugt und zeigt den Wirkungsbereich an.
    - Während der Anzeige des Indikators ist der Anwender für andere Aktionen gesperrt (Skill-Lock).
    - Nach einer festgelegten Verzögerung (ggf. angepasst um Animationsdauer) wird die Skill-Animation abgespielt.
    - Nach Abschluss der Animation wird der Indikator entfernt und der Anwender wieder freigegeben.
2. **Schadensausführung**
    
    - Im definierten Wirkungsbereich vor dem Anwender werden alle Einheiten ermittelt, die Schaden empfangen können.
    - Es wird eine maximale Anzahl an Zielen berücksichtigt, die getroffen werden können.
    - Jeder getroffene Gegner erhält Schaden, der sich aus dem Skillwert und den Attributen des Anwenders berechnet.

---

## ⚙️ Einstellungsoptionen des Nahkampf-Skills

- **Radius:** Wirkungsbereich um den Zielpunkt oder Anwender, in dem Gegner getroffen werden.
- **Schaden:** Basis-Schaden, der auf getroffene Ziele angewendet wird.
- **Layer-Maske:** Bestimmt, welche Objekte als gültige Schadensziele betrachtet werden.
- **Reichweite:** Distanz vor dem Anwender, an der der Wirkungsbereich liegt.
- **Maximale Ziele:** Anzahl der Gegner, die maximal gleichzeitig Schaden erhalten können.
- **Indikator-Prefab:** Visuelle Anzeige des Wirkungsbereichs vor Ausführung des Schadens.
- **Verzögerung (AOE-Delay):** Zeitspanne, die der Indikator vor der Schadensausführung angezeigt wird.

### Heil-Skills

## Bestandteile und Ablauf des Heil-Skills

1. **Skill-Aktivierung**
    
    - Der Anwender aktiviert den Heil-Skill.
    - Im definierten Wirkungsbereich vor dem Anwender werden alle befreundeten Einheiten ermittelt, die leben und von Heilung profitieren können.
    - Falls ein visueller Effekt definiert ist, wird dieser beim allen geheilten Einheiten gespawnt, um die Heilwirkung anzuzeigen.
2. **Heilungsausführung**
    
    - Eine bestimmte Anzahl von Zielen innerhalb des Wirkungsbereichs wird ausgewählt.
    - Jedes Ziel erhält eine Heilung um einen festgelegten Betrag.
    - Optional wird ein spezifischer visueller Effekt auf die geheilten Einheiten angewendet.

---

## ⚙️ Einstellungsoptionen des Heil-Skills

- **Radius:** Wirkungsbereich um den Zielpunkt, in dem Verbündete geheilt werden.
- **Reichweite:** Distanz vor dem Anwender, an der der Wirkungsbereich liegt.
- **Heilmenge:** Betrag der Lebenspunkte, die jedem getroffenen Ziel hinzugefügt werden.
- **Layer-Maske:** Bestimmt, welche Einheiten als gültige Heilziele gelten.
- **Maximale Ziele:** Anzahl der Einheiten, die maximal gleichzeitig geheilt werden können.
- **Heil-Effekt-Prefab:** Visuelle Darstellung der Heilung am Anwender.
- **Visueller Effekt:** Optionaler Effekt, der auf geheilte Einheiten angewendet wird, um die Heilwirkung zu visualisieren.

### Buff- und Debuff-Skills

## Bestandteile und Ablauf des Buff-Skills

1. **Skill-Aktivierung**
    
    - Der Anwender aktiviert den Buff-Skill.
    - Im Wirkungsbereich vor dem Anwender werden Einheiten ermittelt, die lebendig sind und mit Vorteilen (Buffs) beeinflusst werden dürfen.
    - Optional kann der Skill so konfiguriert sein, dass er sich selbst nicht beeinflusst.
2. **Anwendung der Buffs**
    
    - Bis zu einer maximalen Anzahl von Zielen innerhalb des Wirkungsbereichs werden ausgewählt.
    - Auf jedes Ziel werden eine oder mehrere vorgegebene Status-Modifikationen (z.B. Erhöhung von Angriff, Verteidigung, Geschwindigkeit) angewendet.
    - Die Modifikationen werden als positive Effekte (Buffs) angewendet.

---

## Bestandteile und Ablauf des Debuff-Skills

1. **Skill-Aktivierung**
    
    - Der Anwender aktiviert den Debuff-Skill.
    - Im Wirkungsbereich vor dem Anwender werden gegnerische Einheiten ermittelt, die Schaden empfangen können.
2. **Anwendung der Debuffs**
    
    - Bis zu einer maximalen Anzahl von Zielen innerhalb des Wirkungsbereichs werden ausgewählt.
    - Auf jedes Ziel werden eine oder mehrere vorgegebene Status-Modifikationen angewendet.
    - Die Modifikationen werden als negative Effekte (Debuffs) angewendet und können z.B. Angriffskraft, Geschwindigkeit oder Verteidigung reduzieren.

---

## ⚙️ Gemeinsame Einstellungsoptionen von Buff- und Debuff-Skill

- **Radius:** Wirkungsbereich um den Zielpunkt, in dem die Statusänderungen angewendet werden.
- **Reichweite:** Distanz vor dem Anwender, an der der Wirkungsbereich liegt.
- **Maximale Ziele:** Anzahl der Einheiten, die maximal gleichzeitig beeinflusst werden können.
- **Status-Modifikationen:** Liste von vorgefertigten Stat-Änderungen, die auf die Ziele angewendet werden.
- **Faction-Filter (nur Buff):** Möglichkeit, den Buff nur auf Einheiten einer bestimmten Fraktion zu beschränken.
- **Selbstbeeinflussung (nur Buff):** Option, ob der Anwender selbst vom Buff betroffen sein darf.

### Spezielle Skills

# Spezielle Skills – Technische Beschreibung

---

## 🌀 ArenaDashSkill

### 🧩 Bestandteile

- **Radius**: Wirkungsbereich der Flächenangriffe.
- **Damage**: Schaden pro Dash-Angriff.
- **LayerMask**: Zielgruppenfilter für Angriffe.
- **Range**: Nicht direkt verwendet – optional für Erweiterung.
- **DashCount**: Anzahl der aufeinanderfolgenden Dashs.
- **AoeDelay**: Verzögerung vor jedem AOE-Angriff.
- **AoePrefab**: Visueller Indikator und Schadensbereich.

### ⚙️ Ablauf

1. Der Skill sperrt Bewegung und Fähigkeiten des Nutzers.
2. Wiederholt bis zu `DashCount`-mal:
    - Ziele im Umkreis werden gesucht.
    - Das entfernteste Ziel wird bestimmt.
    - Eine visuelle AOE-Vorschau wird am aktuellen Ort platziert.
    - Der Spieler wartet die `AoeDelay` ab.
    - Animation startet.
    - Spieler dash-t zum Ziel mit erhöhter Geschwindigkeit.
3. Abschließend werden Bewegung und Aktionen wieder freigegeben.

---

## 🎯 ArrowSpin

### 🧩 Bestandteile

- **ProjectileDataTemplate**: Enthält alle Informationen über das Projektil (z. B. Schaden, Geschwindigkeit).
- **HeightCorrection**: Vertikale Verschiebung beim Spawnen der Projektile.
- **DelayBetweenProjectiles**: Zeitlicher Abstand zwischen den Projektilen im Spin.

### ⚙️ Ablauf

1. Der Skill startet eine Coroutine.
2. Es werden 8 Projektile in Kreisform rund um den Spieler instanziiert.
3. Jedes Projektil wird leicht zeitversetzt (jeweils um `DelayBetweenProjectiles`) gespawnt.
4. Richtung und Rotation der Projektile basieren auf der Blickrichtung des Spielers.

---

## 👻 InvisibleAoeSkill

### 🧩 Bestandteile

- **Radius**: Schadensreichweite um das Ziel.
- **Damage**: Ausgeteilter Schaden.
- **LayerMask**: Zielgruppenfilter.
- **Range**: Nicht direkt verwendet.
- **AffectedTargets**: Maximal betroffene Ziele.
- **IndicatorPrefab**: Visueller Effekt am Zielort.
- **AoeDelay**: Verzögerung zwischen Positionierung und Schaden.

### ⚙️ Ablauf

1. Der Nutzer wird unsichtbar (inkl. Gesundheitsanzeige & Sprite).
2. Gegner im Umkreis werden gescannt.
3. Ein zufälliges Ziel wird bestimmt.
4. Der Spieler teleportiert sich direkt zu diesem Ziel.
5. Ein AOE-Indikator wird am Zielort platziert.
6. Nach Ablauf der Verzögerung:
    - Animation wird abgespielt.
    - Schaden wird im Wirkungsbereich verursacht.
    - Der Spieler wird wieder sichtbar.

---

## Delayed AOE-Field mit Vorschau und Schaden

# Technische Beschreibung: DelayedAoeField

## Übersicht

DelayedAoeField ist eine Systemkomponente zur Verwaltung von Flächenschadenseffekten (AOE) mit zeitlicher Verzögerung. Die Klasse steuert den kompletten Ablauf eines AOE-Angriffs — von der Anzeige einer Vorwarnung bis zur Schadensauslösung und anschließender Bereinigung.

Typische Anwendungsfälle sind Fähigkeiten wie verzögerte Explosionen, Flächenzauber oder Bossangriffe, bei denen Spieler vor dem Schaden optisch gewarnt werden.

---

## Funktionsweise

1. **Anzeige eines Indikators**  
    Zu Beginn wird ein sichtbarer Indikator auf der Spielwelt erzeugt, der die Wirkungssphäre des späteren Schadens darstellt.
    
2. **Verzögerte Schadensauslösung**  
    Nach einer festgelegten Zeitspanne verschwindet der Indikator, und der Schaden wird auf alle gültigen Ziele im Wirkungsbereich angewendet.
    
3. **AOE-Schadenseffekt**  
    Parallel zur Schadensauslösung kann ein visueller Effekt (z. B. Explosion, Feuer) abgespielt werden, um den Impact zu verdeutlichen.
    
4. **Automatische Bereinigung**  
    Nach der Ausführung zerstört sich das AOE-Objekt selbst, um Ressourcen zu schonen.
    

---

## Einstellbare Parameter

- **DelaySeconds**  
    Die Zeit (in Sekunden), die zwischen Indikatoranzeige und Schadensauslösung vergeht.
    
- **Radius**  
    Die Größe des Schadensbereichs, meist als Kreis definiert. Beispiel: Ein Radius von 3 Metern bedeutet, dass alle Ziele innerhalb von 3 Metern Schaden erleiden.
    
- **Damage**  
    Die Basis-Schadensmenge, die an alle betroffenen Ziele ausgegeben wird.
    
- **MaxTargets**  
    Optional kann die maximale Anzahl an Zielen, die Schaden erhalten, begrenzt werden (z. B. nur die 10 nächsten Gegner).
    
- **AOE-Form**  
    Die Schadenszone kann entweder kreisförmig oder rechteckig (boxförmig) mit Ausrichtung sein. Beispiel: Ein Feuerstoß, der nur vor einem Spieler in einem Kegel Schaden anrichtet.
    
- **Visual Effects**  
    Indikator und Schadenseffekte lassen sich individuell anpassen, um unterschiedliche Fähigkeiten optisch voneinander zu unterscheiden.
    
- **Winkel / Rotation**  
    Bei nicht-kreisförmigen AOEs kann ein Winkel definiert werden, um die Ausrichtung des Schadensfeldes zu steuern.
    

---

## Spielerklassen

Es gibt vier verschiedene Spielerklassen, welche ebenfalls Scriptable Objects sind. Diese funktionieren grundlegend identisch wie die Npcs, nur dass die Skills **inputbasiert** sind und **nicht von der Statemachine beeinflusst** werden.