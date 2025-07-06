#Was ist DILIBAD
**DILIBAD** ist im Rahmen des zweiten Integrationsprojekts des Studiengangs **Social Media Systems** an der **Technischen Hochschule Mittelhessen** entstanden und stellt die **Prüfungs-Endleistung** des Projektteams dar.

## Elevator Pitch

**DILIBAD** ist ein kooperatives **Action-RPG** für bis zu **vier Spieler**, das **Erkundung**, **Ressourcenmanagement**, **Basisbau** und **Verteidigung** zu einem dynamischen Multiplayer-Erlebnis verbindet.

Sammelt wertvolle Kristalle, errichtet eine automatisierte Verteidigungsbasis und trotzt eskalierenden Feindwellen, um das Portal zum finalen Boss zu aktivieren – und die Welt zurückzuerobern.

**Discover. Build. Defend. – DILIBAD: Erobern beginnt mit Zusammenarbeit.**

# Genre und Spielstil

**Genre:**

Kooperatives Action-RPG mit Elementen aus **Base-Building** und **Tower Defense**.

**Plattform:**

Windows (PC)

**Perspektive & Steuerung:**

2D / 2.5D Top-Down mit freier **8-Richtungs-Bewegung**.

Spieler steuern ihre Charaktere mit Maus und Tastatur oder Controller. Kamera folgt dynamisch dem Charakter oder zentriert sich im Koop-Modus ggf. auf das Team.

**Spielstil:**

Das Spiel kombiniert **schnelle, actionreiche Kämpfe** mit **taktischem Aufbau** und **Verteidigung der eigenen Basis** gegen Wellen von Gegnern.

Der Fokus liegt auf **kooperativem Gameplay**, bei dem bis zu 4 Spieler gemeinsam Ressourcen sammeln, Strukturen errichten und sich koordinieren, um Herausforderungen zu bewältigen.

Entscheidungen im Kampf, beim Aufbau und bei der Platzierung von Verteidigungsanlagen haben langfristige Auswirkungen auf den Spielverlauf.

# Steuerung

### Steuerung (General)

**Bewegung:**

- `W`, `A`, `S`, `D` – 8-Richtungs-Bewegung (Top-Down)

**Interaktion:**

- `F` – Interagieren mit Objekten, NPCs, Türen, etc.

**Kampf:**

- `Linke Maustaste` – Skill 1 (z. B. Primärer Angriff)
- `Rechte Maustaste` – Skill 2 (z. B. Spezialangriff)
- `Q` – Skill 3 (z. B. Utility oder Flächenzauber)
- `E` – Skill 4 (z. B. Buff, Dash o.ä.)

# GameLoop und Mechaniken

# **Gameloop – Discover. Build. Defend.**

DILIBAD folgt einem zyklischen Gameplay-Kern, in dem **Sammeln**, **Verteidigen**, **Aufrüsten** und **Kämpfen** miteinander verzahnt sind. Der stetige Wechsel zwischen Ressourcenmanagement, Eskalation und Konfrontation erzeugt eine **natürliche Spannungskurve** – mit dem ultimativen Ziel: den finalen Boss zu besiegen.

## 1️⃣ **Ressourcen sammeln & Aufrüstung**

- Spieler erkunden die Umgebung und **sammeln Kristalle** (blau, grün, selten rot).
- Gesammelte Ressourcen dienen zur:
    - **Charakter-Verbesserung** (Stats wie Schaden, Health, Regeneration)
    - **Basis-Aufrüstung** (Mauern, Ballisten)
- Gleichzeitig sorgt aktives Sammeln und Kämpfen dafür, dass der **Bedrohungslevel (Threat Level)** steigt.

---

## 2️⃣ **Gefahr eskaliert**

- Der **Threat-Level** steigt durch:
    - Gegner töten
    - Kristallabbau
- Bei **100 % Bedrohungsanzeige** wird eine **Angriffswelle** ausgelöst – zunehmend härter mit jeder weiteren Welle.

---

## 3️⃣ **Basisverteidigung**

- Die **angreifenden Wellen** versuchen den **Basiskern** zu zerstören.
- Spieler müssen:
    - **Mauern reparieren oder verbessern**
    - **Ballisten platzieren und aufwerten**
    - **aktiv kämpfen** und sich koordinieren
- Bei Erfolg winkt eine **Belohnung in Form eines roten Schlüsselkristalls**

---

## 4️⃣ **Schlüssel, Portal & Bosskampf**

- **Drei rote Schlüsselkristalle** werden benötigt, um das **Portal zum Bosskampf** zu aktivieren.
- Der Bosskampf findet auf einer **separaten Karte** statt.
- Vorbereitung ist entscheidend: Spieler sollten vor der Aktivierung Upgrades und Ausrüstung maximieren.

---

## 5️⃣ **Sterben & Respawn-Mechanik**

- Bei Tod:
    - **Inventar droppt am Ort des Todes**
    - Spieler respawnen nach kurzer Zeit in der Basis
- Beim **Scheitern im Bosskampf**:
    - Option A: **Game Over**
    - Option B: **Zweite Chance** – Spieler respawnen, aber es folgt eine **verstärkte Angriffswelle** (High-Risk-Phase)

---

## 6️⃣ **Spielende**

- **Sieg:** Finaler Boss wird besiegt – Welt gerettet
- **Niederlage:** Basiskern zerstört **oder** gesamtes Team dauerhaft besiegt

# Ressourcen

**Spielressourcen**

Das Spiel beinhaltet **drei primäre Ressourcen**, die jeweils unterschiedliche Funktionen und Seltenheiten aufweisen. Sie unterscheiden sich in **Farbe, Anwendung und Verfügbarkeit** und unterliegen einem **Seltenheitssystem**.

---

### 🔷 **Blaue Kristalle (Energie-Kristalle)**

- **Funktion:** Hauptressource des Spiels  
- **Verwendung:**
    - Kauf von Spieler-Upgrades  
    - Crafting von Items  
    - Bau und Verbesserung der Basisverteidigung (z. B. Mauern, Ballistae)  
- **Vorkommen:**
    - Weit verbreitet und über die Map verteilt  
    - Können vom Spieler durch Interaktion (Abbau) eingesammelt werden  
- **Besonderheit:** Hauptwährung für Progression und strategische Weiterentwicklung  

---

### 🔴 **Rote Key-Kristalle (Schlüssel-Kristalle)**

- **Funktion:** Progressions-Trigger  
- **Verwendung:**
    - Aktivierung von Boss-Portalen (z. B. Endwellen, Welt-Bosse)  
- **Vorkommen:**
    - Nicht abbaubar  
    - Werden ausschließlich als Belohnung für das Abschließen von Gegner-Wellen (Waves) vergeben  
- **Besonderheit:** Extrem selten, symbolisieren Spielfortschritt  

---

### 🟢 **Grüne Lebens-Kristalle (Vital-Kristalle)**

- **Funktion:** Wiederbelebungsressource  
- **Verwendung:**
    - Wiederbelebung von Spielern  
- **Vorkommen:**
    - Deutlich seltener als blaue Kristalle  
    - Selektiv über die Map verteilt  
- **Besonderheit:** Kritisch für Überleben, besonders in schwierigen Levels oder Bosskämpfen  

---

### **Seltenheitsstufen**

Jede Ressource kann in einer von sechs Seltenheitsstufen vorkommen:

- **Common**  
- **Uncommon**  
- **Rare**  
- **Epic**  
- **Legendary**  
- **Mythic**

Diese beeinflussen:

- Die **Menge** der Ressource, die beim Fund erhalten wird  
- Das **Spawnverhalten**

# Klassen

# Klassenübersicht

In diesem Spiel stehen den Spielern vier unterschiedliche Klassen zur Verfügung, die jeweils einzigartige Rollen und Spielstile bieten:

---

### **Bogenschütze**

- **Rolle:** Fernkampf-DPS  
- **Stärken:** Hoher Schaden auf Distanz, Mobilität  
- **Schwächen:** Geringe Verteidigung und Lebenspunkte  
- **Besonderheiten:**
    - Leicht erhöhte Bewegungsgeschwindigkeit im Vergleich zu anderen Klassen  
    - Eignet sich hervorragend, um Gegner auf Distanz zu halten  
    - Effektiv gegen Einzelziele oder in hit-and-run-Taktiken  

---

### **Ritter**

- **Rolle:** Tank / Nahkampf-Frontkämpfer  
- **Stärken:** Hohe Lebenspunkte und Verteidigung  
- **Schwächen:** Geringe Geschwindigkeit und Mobilität  
- **Besonderheiten:**
    - Kann große Mengen an Schaden absorbieren  
    - Zieht gezielt Gegner an, um das Team zu schützen  
    - Ideal zur Kontrolle des Schlachtfelds und zum Blocken von Zugängen  

---

### **Magier / Heiler**

- **Rolle:** Unterstützer / Debuffer  
- **Stärken:** Heilung, Buffs für Verbündete, Schwächung von Gegnern  
- **Schwächen:** Geringe Verteidigung, moderater bis geringer Schaden  
- **Besonderheiten:**
    - Essenziell für das Überleben des Teams in langen Kämpfen  
    - Kann Verbündete stärken (z. B. Schaden, Verteidigung) und Gegner schwächen (z. B. Verlangsamung, Rüstung senken)  
    - Flexibel einsetzbar als reiner Heiler oder unterstützender Zauberwirker  

---

### **Engineer**

- **Rolle:** Kontroll-/Support-Einheit mit Beschwörungen  
- **Stärken:** Strategische Platzierung von Türmen, Kontrolle des Kampffelds  
- **Schwächen:** Wenig Leben und Nahkampfschaden  
- **Besonderheiten:**
    - Nutzt Türme, um Gegner autonom anzugreifen  
    - Türme können individuell verstärkt werden (z. B. Reichweite, Schaden, Feuerrate)  
    - Benötigt vorausschauendes Platzieren und Management seiner Konstruktionen  

---

Jede Klasse bringt einzigartige Stärken ins Team und ermöglicht strategische Tiefe durch Synergien und Rollenzuteilung.

# Upgrades

### ⚙️ **Upgradesystem**

Das Spiel beinhaltet zwei voneinander getrennte Upgradepfade:

1. **Spieler-Upgrades**  
2. **Gebäude-Upgrades**

Beide Systeme verwenden **blaue Kristalle** als Währung, folgen jedoch unterschiedlichen Upgrade-Logiken.

---

### 🧍‍♂️ **Spieler-Upgrades**

- **Ort:** Upgrade Table (in der Spielwelt platzierbar oder zentral in der Basis verfügbar)  
- **Ziel:** Stetige Verbesserung der Charakterwerte für gesteigerte Überlebens- und Kampfkraft  

### Verbesserbare Stats:

| Stat                      | Effekt                                            |
|---------------------------|--------------------------------------------------|
| **Schaden**               | Erhöht den Output aller Angriffe (Nah- & Fernkampf) |
| **Angriffsgeschwindigkeit** | Reduziert Cooldowns oder Zeit zwischen Angriffen  |
| **Rüstung**               | Verringert eingehenden Schaden                    |
| **Lebenspunkte (Health)** | Erhöht maximale HP                                |
| **Lebensregeneration**    | Stellt regelmäßig HP wieder her (passiv)         |
| **Bewegungsgeschwindigkeit** | Erhöht Mobilität und Ausweichmöglichkeiten       |

### Upgrade-System:

- Jeder Stat hat eine **Maximalstufe**  
- Die **Kosten** steigen mit jeder Stufe nach einer **nicht-linearen Kurve**

---

### 🏰 **Gebäude-Upgrades**

- **Ort:** Direkt an der Basis beim Platzieren oder Interagieren mit Verteidigungsanlagen  
- **Ziel:** Verbesserung der defensiven Infrastruktur zum Schutz gegen Gegner-Waves  

### Upgradefähige Gebäude:

1. **Ballista**  
    - **Leben:** Erhöht die Haltbarkeit gegen gegnerische Angriffe  
    - **Schaden:** Erhöht den verursachten Schaden gegen Gegner  
2. **Mauer**  
    - **Leben:** Steigert die Widerstandsfähigkeit und Haltbarkeit  

### Upgrade-Logik:

- Auch hier werden **blaue Kristalle** als Ressource verwendet  
- Upgrades sind **modular**: jede platzierte Einheit (z. B. einzelne Mauersegmente oder Ballisten) wird separat verbessert  
- Auch hier: **steigende Kostenkurve** je Stufe


# Inventar, Items und Crafting

Das Spiel verwendet **zwei Inventararten**, die unterschiedliche Funktionen erfüllen:

---

### 🧱 **1. Basis-Karren (Gemeinschaftsinventar)**

- **Ort:** Zentraler Karren in der Spielbasis  
- **Funktion:**
    - Spieler können **Kristalle (blau, grün, rot)** abgeben  
    - Dient zur **Finanzierung von Base-Upgrades**, **Bossportal-Aktivierung** und **kollektiven Fortschritt**  
- **Designziel:** Kooperatives Ressourcenmanagement, fördert Teamplay und Risikoabwägung  

---

### 🎮 **2. Spielerinventar**

- **Kapazität:** 8 Slots pro Spieler  
- **Regeln:**
    - **Kristalle stacken** (z. B. bis **100 Einheiten pro Slot**)  
    - **Equipables** (Ausrüstung) belegen **1 Slot** und **stacken nicht**  
- **Funktion:**
    - Sammlung von Ressourcen  
    - Items können durch Rechtsklick auf das Icon fallen gelassen werden  
    - Tragen von Ausrüstung (Equipables)  
    - Taktisches Management zwischen Loot, Crafting, Risiko  

### ⚠️ **Tod & Inventarverlust**

- Stirbt ein Spieler, lässt er sein komplettes Inventar am Ort des Todes fallen  
- Die Items und Kristalle bleiben als **Loot-Paket** in der Welt liegen  
- **Andere Spieler** können sie aufsammeln oder verteidigen, bis der tote Spieler zurückkehrt  

---

### 🛠️ **Crafting- & Equip-System**

- **Zugang:** Am Upgrade Table oder eigenem Crafting-Tisch  
- **Verwendung:** Herstellung von **Equipable Items**, die mehrere **Stat-Boni gleichzeitig** verleihen  
- **Kosten:** 25 blaue Kristalle  
- **Einschränkung:** Jedes Item belegt 1 Inventar-Slot und kann **nur einmal getragen** werden  

# NPCs

# Gegnerfraktionen

Aktuell sind drei unterschiedliche Gegnerfraktionen implementiert:

### Northmarchborn

Barbarenartige Gegner mit Fokus auf Nahkampf und Fernkampfangriffe:

- Standardgegner: Barbar, Bogenschütze, Nomade, Hexendoktor, Elite-Bogenschütze, Berserker
- Miniboss: Oger
- Hauptboss: Großer, golemartiger Barbar

### Graveborn

Skelett-basierte Gegner mit vielfältigen Klassen:

- Standardgegner: Skelettkrieger
- Weitere Gegner: Bogenschütze, Magier, Berserker
- Miniboss: Skelett-Ritter
- Hauptboss: Großes, brutales Skelett

### Soulforge

Dunkle Skelettfraktion mit besonderen Fähigkeiten:

- Standardgegner: Dunkler Bogenschütze, Nekromant
- Miniboss: Todeslord (großes Skelett mit Rüstung und flammendem Schwert)
- Hauptboss: Todesritter mit Unsichtbarkeits- und Überraschungsangriffsfähigkeiten

## Besondeheit: Gigantischer Boss

Ein einzigartiger Endboss mit speziellen Animationen und Fähigkeiten. Statt sich frei zu bewegen, agiert er als stationäres CombatBuilding. Seine Angriffe umfassen großflächige Schläge auf die Arena, z.B. durch Zuschlagen mit den Armen oder Zusammenschlagen der Hände, um massive Schadenszonen zu erzeugen.

---

# NPC State Machine

Jeder NPC im Spiel wird durch eine State Machine gesteuert, die sieben verschiedene Zustände (States) umfasst:

- **Idle State:**
    
    Die NPCs haben kein aktuelles Ziel, bewegen sich nicht und befinden sich in einem Ruhezustand. Während dieses States steigt ein "Langeweile"-Wert, der nach Erreichen eines bestimmten Schwellenwerts den Wechsel in den Wandering State auslöst.
    
- **Wandering State:**
    
    NPCs bewegen sich zu einem neuen Zielpunkt, um ihre Umgebung zu erkunden und lebendiger zu wirken. Dieser State legt lediglich das Bewegungsziel fest, das eigentliche Bewegen wird im Movement State ausgeführt.
    
- **Movement State:**
    
    Verantwortlich für die generelle Fortbewegung der NPCs zum festgelegten Zielpunkt.
    
- **PickSkill State:**
    
    Die NPCs wählen in regelmäßigen Abständen eine Fähigkeit aus und bereiten diese vor.
    
- **Attack State:**
    
    In diesem State führen die NPCs die ausgewählten Fähigkeiten aus und bewegen sich dabei aktiv auf Spieler zu.
    
- **Enrage State (boss-spezifisch):**
    
    Dieser State stellt einen Damage-Check dar. Nach Entdeckung des ersten Spielers startet ein Timer. Läuft dieser ab, ohne dass der Boss besiegt wurde, wird ein mächtiger Angriff ausgelöst, der die gesamte Arena betrifft. Die Spieler haben eine letzte Chance, während der Ladephase des Angriffs, den Boss mit maximalem Schaden zu besiegen. Schafft der Boss es, den Angriff durchzuführen, wird die gesamte Spielergruppe besiegt. Dieser Mechanismus sorgt für erhöhte Spannung und fordert gutes Teamplay.
    
- **Invulnerability (Invuln) State (boss-spezifisch):**
    
    In bestimmten Lebensabschnitten wird der Boss unbesiegbar und führt eine Weltaktion durch — eine Mechanik, die von den Spielern Ausweichmanöver erfordert. Nach Abschluss dieser Phase ist der Boss wieder verwundbar.
    

---

# Skilltypen und deren Einsatz

## Projektil-Skills

Ermöglichen das Abschießen von Geschossen in verschiedene Richtungen. Hauptsächlich von Bogenschützen und Magiern genutzt. Spieler können gezielt auf Gegner schießen, während Gegner automatisierte Zielverfolgung verwenden.

## Turret- und Deployable-Skills

Skills zum Platzieren stationärer Objekte wie Türme, die eine bestimmte Zone kontrollieren und Schaden verursachen. Hauptsächlich vom Engineer-Spielercharakter verwendet.

## Flächen-Effekte (AoE-Skills)

Fähigkeiten, die an einer Zielposition aktiviert werden und Schaden in einem festgelegten Radius verursachen. Beispielsweise von Rittern zur Schadensverteilung an mehreren Gegnern genutzt.

## Nahkampf-Skills

Nahbereichsangriffe mit Animationen und zeitlichen Verzögerungen. Diese Fähigkeiten sind Standard für Nahkampfeinheiten und fördern taktische Nahkampfsituationen. Variationen mit stärkeren Angriffen gibt es bei Engineer und Ritter.

## Heil-Skills

Fähigkeiten zur Regeneration von Lebenspunkten bei Verbündeten im Wirkungsbereich. Unterstützen Teamplay und Überlebensfähigkeit, meist vom Magier/Heiler eingesetzt.

## Buff- und Debuff-Skills

Temporäre Verstärkung von Verbündeten (Buffs) oder Einschränkung von Gegnern (Debuffs). Magier/Heiler nutzen diese Fähigkeiten, um das Team zu stärken und Gegner zu schwächen. Der Engineer verwendet Buffs, um seine Türme zu verbessern.

## Spezielle Skills

Einzigartige Fähigkeiten, z.B.:

- Unsichtbarer AoE-Angriff: Teleportation zum nächsten Gegner gefolgt von einem AoE-Schaden (Boss-spezifisch).
- Dash-Angriff: Schnelles Zuschlagen beim weit entferntesten Gegner (z.B. Golem).
- Kreisförmiger Projektilangriff: Mehrere Pfeile werden in einer kreisförmigen Formation abgefeuert, genutzt von Bogenschützen und in höheren Schwierigkeitsgraden auch von Gegnern.

---

Diese vielfältigen Skilltypen bieten ein breites taktisches Spektrum und sorgen für abwechslungsreiche, dynamische Kampfmechaniken. Jeder Boss besitzt einzigartige Fähigkeiten, die seine Herausforderung und Persönlichkeit unterstreichen.


# Waves und Bossfight

### **Wave-System & Bedrohungsmechanik**

Das Spiel verwendet ein dynamisch ausgelöstes **Wave-System**, bei dem feindliche Angriffe auf die Spielerbasis stattfinden. Die Auslösung der Waves hängt direkt mit dem **Bedrohungslevel** (Threat Level) zusammen, das durch Spieleraktionen beeinflusst wird.

---

### **Waves – Angriffswellen**

- **Definition:** Eine Wave besteht aus **mehreren Gegnern**, die **in der Nähe der Basis** spawnen und das **Ziel verfolgen, die Basis zu zerstören**.  
- **Folgen bei Scheitern:** Wird die Basis zerstört, tritt ein **Game Over** ein.  
- **Gegnerzusammensetzung:**  
    - Hauptsächlich reguläre Feinde  
    - Zusätzlich **stärkere Eliteeinheiten** oder Mini-Bosse als besondere Bedrohungskomponente  

### **Threat-Level-System (Bedrohungslevel)**

- **Funktion:** Kontrolliert, **wann eine Wave ausgelöst** wird  
- **Steigt durch bestimmte Spieleraktionen:**  
    - Töten von Gegnern  
    - Abbau von Kristallen (insbesondere in großen Mengen)  
- **Threshold-System:** Bei Erreichen eines bestimmten Schwellenwerts **löst sich automatisch die nächste Wave aus**  

**Threat-Level sinkt nicht passiv**, bleibt also als Druckmittel erhalten

### **Belohnungssystem**

- Nach erfolgreichem Abschluss einer Wave erhalten **alle Spieler gemeinsam** genau **einen roten Key-Kristall**  
- Diese Kristalle sind **selten und nicht farmbar**, sondern nur durch erfolgreiche Verteidigung erhältlich  
- **Verwendung:** Aktivieren von **Bossportalen** oder Zugang zu neuen Spielabschnitten  

### **Designintention**

- Kombiniert **Risikomanagement** (Basis vs. Bedrohung) mit **Belohnungssystem**  
- Fördert **kooperatives Spielverhalten**, da Ressourcen und Bedrohung geteilt werden  
- Baut eine natürliche **Gameplay-Dramaturgie** auf: Ressourcenabbau = Machtgewinn, aber auch = Gefahr  

### Bosskämpfe

**Allgemeiner Ablauf:**

Der Spieler kann über spezielle Teleporter innerhalb der Hauptwelt („Zweigwald“) optionale Bossarenen betreten. Der Zugang zu diesen Arenen ist **kostenpflichtig** und erfordert das **Einlösen von Schlüssel-Kristallen**, einer limitierten und wertvollen Ressource im Spiel.

- Die **Bossauswahl ist zufällig oder rotierend** – bei der Aktivierung des Teleporters wird **nicht im Voraus bekanntgegeben**, welchem Boss sich der Spieler stellen muss.  
- Jeder Bosskampf findet in einer **eigenständigen Arena-Map** statt, losgelöst von der Hauptwelt.  
- Nach einem erfolgreichen Kampf wird dem Spieler **die Rückkehr in die Hauptwelt gestattet**. Ein Scheitern führt zum Game Over oder Neustart der Sequenz (abhängig vom Schwierigkeitsgrad oder Spielmodus).  
- Die Bosskämpfe dienen dem Fortschritt, bieten Belohnungen und erschließen neue Fähigkeiten oder Ressourcen.  

**Finaler Bosskampf:**

Im letzten Spielabschnitt, kurz vor Spielende, wird der Spieler in einen **fest definierten Bosskampf** geführt. Dieser ist zentraler Bestandteil der Hauptstory und unterscheidet sich in Inszenierung, Mechanik und Bedeutung deutlich von den optionalen Bossen zuvor.

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

# Art Style & Asset-Nutzung

3.1 Stilistische Ausrichtung

Für das Projekt wurde bewusst ein **2,5D-Artstyle** gewählt. Diese Entscheidung basiert auf der Zielsetzung, einen stilisierten, aber ressourcenschonenden Look zu erreichen, der sowohl atmosphärisch als auch technisch gut skalierbar ist.

### 3.2 Asset-Auswahl und -Strategie

Um Entwicklungszeit und -aufwand effizient zu nutzen, wurde auf **bestehende Asset-Pakete aus dem Unity Asset Store** zurückgegriffen. Das erlaubt es dem Team, sich stärker auf Gameplay, Level-Design und Systementwicklung zu konzentrieren, ohne große Kapazitäten für eigene grafische Inhalte aufbringen zu müssen.

### Karten- und Umgebungsgestaltung

Für das Map-Design wurden Assets des Publishers **Rafael Matos (Epic RPG World)** verwendet. Diese Pakete bieten eine hohe Vielfalt an Szenarien (z. B. Vulkan, Ruinen, Graslandschaften), während der **visuelle Stil durchgehend konsistent** und gut kombinierbar bleibt.

**Verwendete Pakete:**

- [ERW – The Depths V1.5.1](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-the-depths-of-the-mountain-282389)
- [ERW – Ancient Ruins V1.9.1](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-ancient-ruins-268309)
- [ERW – Grass Land V1.5.1](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-grass-land-legacy-268312)
- [ERW – Grassland 2.0 V1.7.2](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-grass-land-2-0-267533)
- [ERW – Volcano V1.6](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-volcano-310290)

### Charaktere und Gegner

Für die spielbaren Charaktere und Gegner wurden mehrere Pakete von **SmallScaleInteractive** und weiteren Anbietern verwendet. Diese Assets sind **stilistisch kompatibel**, animiert und decken ein breites Spektrum an Charakterklassen und Gegnerfraktionen ab.

**Spielbare Charaktere:**

- [TopDown HD Character Pack – Animated 2D Pixel Characters](https://assetstore.unity.com/packages/2d/characters/topdown-hd-character-pack-animated-2d-pixel-characters-282727)*(Knight, Archer, Mage)*

**Engineer-Klasse:**

- [TopDown HD Enemy Pack](https://assetstore.unity.com/packages/2d/characters/topdown-hd-enemy-pack-305615)

**Gegnerfraktionen:**

- [TopDown HD Barbarian Pack – Animated 2D Pixel Characters](https://assetstore.unity.com/packages/2d/characters/topdown-hd-barbarian-pack-animated-2d-pixel-characters-286445)
- [TopDown HD Undead Pack – Animated 2D Pixel Characters](https://assetstore.unity.com/packages/2d/characters/topdown-hd-undead-pack-animated-2d-pixel-characters-286619)
- DeathKnight: ebenfalls aus dem TopDown HD Character Pack
- BigBoss: visuell eingebettet durch Assets aus *ERW – Volcano V1.6*

**Weitere Quellen:**

- [SmallScaleInteractive – Character Creator: Fantasy](https://assetstore.unity.com/packages/2d/characters/character-creator-fantasy-320564)
Der Charakter-Editor wurde getestet und bietet viel Potenzial für zukünftige Erweiterungen. Aufgrund des hohen Importaufwands und der Vielzahl an Konfigurationsmöglichkeiten wurde jedoch entschieden, ihn in der aktuellen Produktion nicht zu verwenden. Er ist für spätere Updates eingeplant.

**Eigene Assets:**

- Die Turret-Grafiken des Engineer-Charakters wurden in Eigenarbeit von **Pascal Meier** erstellt.

### 3.3 Konvertierung von 3D in 2D – Proof of Concept

Um das grafische Repertoire zu erweitern, wurde experimentell getestet, ob **3D-Assets in 2D-Pixelstil konvertiert** werden können. Dies geschah mithilfe des Tools:

- [Pixelate – Pixel Art Converter](https://assetstore.unity.com/packages/tools/sprite-management/pixelate-pixel-art-converter-194727)

Ein Beispiel hierfür ist die **Ballista aus dem POLYGON Fantasy Kingdom Pack von Synty**:

[Link](https://assetstore.unity.com/packages/3d/environments/fantasy/polygon-fantasy-kingdom-low-poly-3d-art-by-synty-164532)

### Probleme & Erkenntnisse

Ein technisches Problem trat auf, als 3D-Waffen (z. B. Schwerter) an Charaktere gebunden wurden. Diese bewegten sich verzögert gegenüber den Bones, wodurch die Waffen während bestimmter Animationen in der Luft "schwebten".

Nach eingehender Recherche wurde festgestellt, dass die Bone-Zuweisung in Unity anders aktualisiert wird als erwartet.

**Lösung (nicht final implementiert):**

In **Blender** können Waffen direkt einem bestimmten Bone (z. B. der Hand) zugewiesen werden, was das Problem behebt. Dieses Vorgehen wurde aus Zeitgründen jedoch nicht weiterverfolgt und wird für einen späteren Entwicklungszeitpunkt aufgeschoben.

### 3.4 Ausblick

Die bisherigen Tests zeigen, dass mit der Kombination aus bestehenden 2D-Paketen und konvertierten 3D-Modellen **eine große visuelle Vielfalt** erreicht werden kann. Insbesondere für zukünftige Charaktererweiterungen und Boss-Designs bietet sich die Kombination aus Shader-Techniken, Bone-Anpassung und Stilangleichung als praktikabler Weg an, um konsistenten Content im gewählten Artstyle zu generieren.

## 3. Art Style & Asset-Nutzung

### 3.1 Stilistische Ausrichtung

Für das Projekt wurde bewusst ein **2D-Artstyle** gewählt. Diese Entscheidung basiert auf der Zielsetzung, einen stilisierten, aber ressourcenschonenden Look zu erreichen, der sowohl atmosphärisch als auch technisch gut skalierbar ist.

### 3.2 Asset-Auswahl und -Strategie

Um Entwicklungszeit und -aufwand effizient zu nutzen, wurde auf **bestehende Asset-Pakete aus dem Unity Asset Store** zurückgegriffen. Das erlaubt es dem Team, sich stärker auf Gameplay, Level-Design und Systementwicklung zu konzentrieren, ohne große Kapazitäten für eigene grafische Inhalte aufbringen zu müssen.

#### Karten- und Umgebungsgestaltung

Für das Map-Design wurden Assets des Publishers **Rafael Matos (Epic RPG World)** verwendet. Diese Pakete bieten eine hohe Vielfalt an Szenarien (z. B. Vulkan, Ruinen, Graslandschaften), während der **visuelle Stil durchgehend konsistent** und gut kombinierbar bleibt.

**Verwendete Pakete:**
- [ERW – The Depths V1.5.1](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-the-depths-of-the-mountain-282389)
- [ERW – Ancient Ruins V1.9.1](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-ancient-ruins-268309)
- [ERW – Grass Land V1.5.1](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-grass-land-legacy-268312)
- [ERW – Grassland 2.0 V1.7.2](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-grass-land-2-0-267533)
- [ERW – Volcano V1.6](https://assetstore.unity.com/packages/2d/environments/epic-rpg-world-volcano-310290)

#### Charaktere und Gegner

Für die spielbaren Charaktere und Gegner wurden mehrere Pakete von **SmallScaleInteractive** und weiteren Anbietern verwendet. Diese Assets sind **stilistisch kompatibel**, animiert und decken ein breites Spektrum an Charakterklassen und Gegnerfraktionen ab.

**Spielbare Charaktere:**
- [TopDown HD Character Pack – Animated 2D Pixel Characters](https://assetstore.unity.com/packages/2d/characters/topdown-hd-character-pack-animated-2d-pixel-characters-282727)  
  _(Knight, Archer, Mage)_

**Engineer-Klasse:**
- [TopDown HD Enemy Pack](https://assetstore.unity.com/packages/2d/characters/topdown-hd-enemy-pack-305615)

**Gegnerfraktionen:**
- [TopDown HD Barbarian Pack – Animated 2D Pixel Characters](https://assetstore.unity.com/packages/2d/characters/topdown-hd-barbarian-pack-animated-2d-pixel-characters-286445)
- [TopDown HD Undead Pack – Animated 2D Pixel Characters](https://assetstore.unity.com/packages/2d/characters/topdown-hd-undead-pack-animated-2d-pixel-characters-286619)
- DeathKnight: ebenfalls aus dem TopDown HD Character Pack  
- BigBoss: visuell eingebettet durch Assets aus *ERW – Volcano V1.6*

**Weitere Quellen:**
- [SmallScaleInteractive – Character Creator: Fantasy](https://assetstore.unity.com/packages/2d/characters/character-creator-fantasy-320564)  
  Der Charakter-Editor wurde getestet und bietet viel Potenzial für zukünftige Erweiterungen. Aufgrund des hohen Importaufwands und der Vielzahl an Konfigurationsmöglichkeiten wurde jedoch entschieden, ihn in der aktuellen Produktion nicht zu verwenden. Er ist für spätere Updates eingeplant.

**Eigene Assets:**
- Die Turret-Grafiken des Engineer-Charakters wurden in Eigenarbeit von **Pascal Meier** erstellt.

### 3.3 Konvertierung von 3D in 2D – Proof of Concept

Um das grafische Repertoire zu erweitern, wurde experimentell getestet, ob **3D-Assets in 2D-Pixelstil konvertiert** werden können. Dies geschah mithilfe des Tools:

- [Pixelate – Pixel Art Converter](https://assetstore.unity.com/packages/tools/sprite-management/pixelate-pixel-art-converter-194727)

Ein Beispiel hierfür ist die **Ballista aus dem POLYGON Fantasy Kingdom Pack von Synty**:  
[Link](https://assetstore.unity.com/packages/3d/environments/fantasy/polygon-fantasy-kingdom-low-poly-3d-art-by-synty-164532)

#### Probleme & Erkenntnisse

Ein technisches Problem trat auf, als 3D-Waffen (z. B. Schwerter) an Charaktere gebunden wurden. Diese bewegten sich verzögert gegenüber den Bones, wodurch die Waffen während bestimmter Animationen in der Luft "schwebten".

Nach eingehender Recherche wurde festgestellt, dass die Bone-Zuweisung in Unity anders aktualisiert wird als erwartet.

**Lösung (nicht final implementiert):**  
In **Blender** können Waffen direkt einem bestimmten Bone (z. B. der Hand) zugewiesen werden, was das Problem behebt. Dieses Vorgehen wurde aus Zeitgründen jedoch nicht weiterverfolgt und wird für einen späteren Entwicklungszeitpunkt aufgeschoben.

### 3.4 Ausblick

Die bisherigen Tests zeigen, dass mit der Kombination aus bestehenden 2D-Paketen und konvertierten 3D-Modellen **eine große visuelle Vielfalt** erreicht werden kann. Insbesondere für zukünftige Charaktererweiterungen und Boss-Designs bietet sich die Kombination aus Shader-Techniken, Bone-Anpassung und Stilangleichung als praktikabler Weg an, um konsistenten Content im gewählten Artstyle zu generieren.



# Sound-Design

## Überblick  
Sound ist ein zentrales Gestaltungselement unseres Spiels. Er erzeugt Atmosphäre, unterstützt die Immersion und hilft den Spielern, sich in der Welt zurechtzufinden. Unser Ziel ist es, den Spielern durch gezielte Klänge und Musik das Gefühl zu vermitteln, wirklich Teil dieser Welt zu sein – vom ruhigen Basislager bis zum dramatischen Bosskampf. Jeder Sound wurde mit Bedacht gewählt und im Kontext getestet, um eine stimmige und funktionale Klangwelt zu schaffen.

---

## Atmosphäre & Klangwelten  

### Die Basis  
In der Heimatbasis herrscht Ruhe. Eine sanfte Hintergrundmusik vermittelt Sicherheit und gibt den Spielern Zeit zum Durchatmen. Dezente Klick- und Navigationsgeräusche im UI machen die Bedienung greifbar und geben sofort Rückmeldung auf Aktionen.

**Wirkung:**  
- Gefühl von Kontrolle  
- Kontrast zur Action in der Spielwelt  
- Erholungspunkt zwischen Kämpfen  

---

### Die Welt  
Außerhalb der Basis wird die Klangkulisse lebendiger: dezente Windgeräusche, Vogelstimmen oder kristallines Flirren begleiten die Reise durch offene Gebiete. Beim Kristallabbau sorgen spezifische Mining-Geräusche für direktes Feedback und verstärken das Gefühl, mit der Welt zu interagieren.

**Wirkung:**  
- Naturhaftigkeit und Weite  
- Spieler spüren durch den Sound, wenn sie etwas Bedeutendes tun  
- Audio-Hinweise leiten subtil durch die Spielwelt  

---

### Der Kampf  
Im Kampfgeschehen sind Fähigkeitensounds direkt, klar und kraftvoll gestaltet, damit Aktionen nachvollziehbar bleiben. Jeder Skill erhält ein akustisches Profil, das ihn nicht nur effektiv, sondern auch befriedigend klingen lässt.

**Wirkung:**  
- Steigerung der Spannung  
- Jeder Skill fühlt sich durch Sound-Feedback „mächtig“ an  
- Orientierung im Chaos des Gefechts  

---

### Bosskämpfe  
Jede Bossarena besitzt eine eigene, dichte Klangwelt. Die Musik ist orchestral, bedrohlich und atmosphärisch intensiv. Jeder Boss hat ein unverwechselbares akustisches Motiv, das bereits beim Betreten der Arena Spannung aufbaut. Soundeffekte von Bossangriffen sind meist basslastig und raumfüllend – sie unterstreichen Größe und Bedrohung.

**Wirkung:**  
- Gänsehaut-Effekt beim Arena-Betreten  
- Akustische Identität für jeden Boss  
- Unterstützung der Dramatik und Bedeutung  

---

## UI & Feedback  
Auch Menüs, Buttons und Systemmeldungen wurden bewusst vertont. Ein Klick auf einen Button löst einen dezenten „Tap“-Sound aus. So entsteht ein konsistentes, hochwertiges Interface-Erlebnis.

**Wirkung:**  
- Schnelle Rückmeldung auf Aktionen  
- Erhöhung der Bedienfreundlichkeit  
- Klangliche Konsistenz im gesamten Spiel  

---

## Klanggestaltung & Auswahl  
Alle Klänge wurden so gewählt, dass sie sowohl zur Spielmechanik als auch zur gewünschten Stimmung passen. Kein Sound wirkt zufällig – jeder hat eine spezifische Funktion im Erlebnisdesign. Wiedererkennbarkeit, Einbindung in die Spielwelt und funktionale Klarheit standen im Fokus der Entwicklung.

---

## Zukunft & Erweiterbarkeit  
Das Sound-System ist modular und skalierbar aufgebaut, sodass es problemlos um weitere Klanglandschaften ergänzt werden kann. Geplant sind zukünftige Erweiterungen wie:

- **Dynamischere Übergänge**, z. B. Musikwechsel je nach Tageszeit oder Spielsituation  
- **Spezifische Ambients** für neue Regionen wie düstere Dungeons oder bizarre Zwischenräume  
- **Mehr musikalische Tiefe** durch Layered Audio und adaptive Musik  
- **Kontextuelle Sounds**, die auf Spieleraktionen und Umgebung reagieren  

---

## Visualisierung & Arbeitsweise  
Zur Verdeutlichung unserer Arbeit mit dem Sound-System sind folgende Beispielbilder geplant:

- Radiusanzeige eines 3D-Sounds im Editor  
- Auswahl eines Sounds in der zentralen Soundverwaltung  
- Beschrifteter Inspektor einer UI-Sound-Komponente  
- Musik beim Kampf – Beispielkomposition aus Ableton  

# Story und Lore

In der instabilen Zwischenwelt **Uitta**, einem Ort jenseits von Raum und Zeit, treffen vier Helden aus unterschiedlichen Dimensionen aufeinander. Sie stammen aus verschiedenen Epochen und Realitäten, doch wurden sie alle durch chaotische Kräfte aus ihren Welten gerissen und hierher versetzt.

**Uitta** ist ein Ort voller magischer Instabilität. Sein **Kristallkern** verzerrt Zeit und Raum, lässt Portale entstehen und verändert die Naturgesetze. Die Welt ist geprägt von schwankenden Biomen, fremdartigen Energien und dem Einfluss zweier Monde, die Gravitation und Wetter stetig verändern.

Räuberische Fraktionen aus geplünderten Dimensionen durchstreifen das Land auf der Suche nach Ressourcen, Kristallen und neuer Macht. Gleichzeitig haben sich verstreute Gruppen freundlicher Überlebender angesiedelt, die in kleinen Siedlungen Schutz, Wissen oder Hilfe anbieten.

Im Zentrum des Geschehens errichten die vier Fremden eine improvisierte Basis an einem alten Fort – ein Ausgangspunkt für ihre Suche nach Antworten, Hoffnung und dem Weg zurück in ihre zerrissenen Heimaten.

# Map Design

## 5. Map Design

### 5.2 Kartenübersicht

Das Spiel verwendet fünf Karten teils mit Varianten, um Abwechslung und unterschiedliche Spielerfahrungen zu ermöglichen:

- **Der Zweigwald**
- **Die Verfallene Stadt**
- **Das Sonnenlabyrinth**
- **Great Depths** (horizontaler Fokus)
- **Smaller Depths** (vertikaler Fokus)
- **Lava-Kammer**
- **Barbarenlager**

## Der Zweigwald *(Hauptwelt)*

verwendete Asseets:

- Ancient Ruins
- Grassland V1
- Grassland V2

### 1. Ziel des Map Designs

Der **Zweigwald** fungiert als offene Hauptwelt und zentraler Knotenpunkt des Spiels. Ziel ist es, eine erkundbare Umgebung mit klar voneinander abgrenzbaren Regionen zu gestalten, die dem Spieler Orientierung, Entscheidungsfreiheit und sukzessiven Fortschritt ermöglichen.

### 2. Struktur und Layout

Die Karte ist in zwei kontrastierende Biome aufgeteilt:

- Ein **rötlicher Westteil**, geprägt von herbstlicher Vegetation und felsiger Topografie.
- Ein **bläulicher Ostteil**, der kühler und mystischer wirkt.

Mehrere Pfade durchziehen die Karte – teils offensichtliche Wege, teils versteckte Routen. Das Zentrum des Zweigwalds dient als **Hub** für den Übergang in andere Welten.

### 3. Gameplay-Elemente

- Versteckte Pfade und kleinere Umwege
- Leichte Rätsel zur Auflockerung des Tempos
- Kleinere Gegnergruppen zur Aktivierung der Kampfmechanik
- Storyeinschübe an markanten Orten

Fokus: **Erkundung**, **Bewegung**, **narrative Integration**

### 4. Technisches Map-Layout

Die Map ist **horizontal** orientiert, mit sukzessiver Freischaltung von **Querverbindungen**. Das Layout folgt einem **offenen, aber geführten Design**, wobei das Navigationsnetzwerk bewusst mit Abkürzungen und Rückwegen arbeitet.

### 5. Designprinzipien & Spielerführung

- Farbgebung und Biome dienen der groben Orientierung
- Subtile visuelle Hinweise (z. B. Lichtquellen, Blickachsen, Landmarken) helfen bei der Pfadwahl
- Zentrale Strukturen wie Türme, Statuen oder Ruinen fungieren als Wegweiser

### 6. Map-spezifische Systeme

- **Zentrales Portal** als Zugang zu anderen Karten
- **Freischaltbare Rückverbindungen** zur Förderung von Backtracking und non-linearem Fortschritt

### 7. Kartenbeispiele

- Frühe Skizzen der Weltaufteilung
- Finale Top-Down-Karte mit Biomen und Pfaden

### 8. Zusammenhang mit Spielmechaniken

Das Leveldesign fördert einen explorativen Spielstil, ohne Spieler zu überfordern. Die Wege sind so konzipiert, dass verschiedene Spielweisen möglich sind – etwa vorsichtiges Erkunden, gezieltes Voranschreiten oder das gezielte Suchen nach versteckten Inhalten.

---

## Die Verfallene Stadt

verwendete Asseets:

- Ancient Ruins

### 1. Ziel des Map Designs

Die Verfallene Stadt ist ein **instanziiertes Gebiet** mit **engem, labyrinthartigem Aufbau**. Ziel ist es, durch unterschiedliche Startpunkte jedes Mal eine neue, variierende Spielerfahrung zu erzeugen.

### 2. Struktur und Layout

Die Karte ist **zentrisch aufgebaut**, mit **Startpunkten in allen vier Ecken**. Jeder Pfad zur Mitte unterscheidet sich in Komplexität, Richtung, Gegneraufkommen und Rhythmus.

### 3. Gameplay-Elemente

- Unterschiedliche Gegnerverteilung je nach Startpunkt
- Variable Itempositionen
- Kämpfe in engen Gassen
- Vereinzelte Rückwege und Schleichpassagen

Fokus: **Pfadwahl**, **Kampfvariabilität**, **Positionsspiel**

### 4. Technisches Map-Layout

Das Layout besteht aus **verzweigten Gassen mit Kreuzungen und Sackgassen**, die unterschiedliche Wege zur Mitte ermöglichen. Alternative Routen eröffnen sich durch Erkundung oder Aktionen im Spielverlauf.

### 5. Designprinzipien & Spielerführung

- Die Struktur zieht den Spieler automatisch zur Mitte
- Farbgebung, architektonische Elemente und zerstörte Landmarken geben Orientierung
- Ein steigender Schwierigkeitsgrad führt zu einem Spannungsbogen in Richtung Zentrum

### 6. Map-spezifische Systeme

- **Kooperatives Szenario** möglich: Spieler starten getrennt und müssen sich in der Mitte treffen
- **Dynamische Routenöffnung**: Neue Wege erscheinen durch Spielfortschritt oder Umwelteinflüsse

### 7. Kartenbeispiele

- Skizzen mit vier Einstiegspunkten
- Finales Leveldiagramm mit alternativen Routen und Begegnungszonen

### 8. Zusammenhang mit Spielmechaniken

Die Karte nutzt instanziierte Herausforderungen, um gezielt Wiederspielwert zu erzeugen. Durch variierende Pfade und sich verändernde Spielbedingungen passt sich das Level verschiedenen Spielstilen an (z. B. stealth vs. actionorientiert).

---

## Das Sonnenlabyrinth

verwendete Asseets:

- Grassland V1
- Grassland V2

### 1. Ziel des Map Designs

Das Sonnenlabyrinth ist eine **kreisförmig aufgebaute Puzzle-Map**, deren Ziel es ist, die Mitte über verschiedene Wege zu erreichen. Jeder Run soll sich durch neue Routen und Blocker unterscheiden.

### 2. Struktur und Layout

Vier Startpunkte an den Kardinalpunkten führen in ein **konzentrisches, spiralförmiges Labyrinth**. Die Wege unterscheiden sich deutlich, was die Wiederholung abwechslungsreich gestaltet.

### 3. Gameplay-Elemente

- Rätselpassagen mit interaktiven Mechanismen
- Versteckte Abkürzungen und temporäre Blocker
- Gelegentliche Gegner oder Fallen zur Rhythmusbrechung

Fokus: **Rätsellogik**, **Pfadoptimierung**, **Exploration unter Zeitdruck**

### 4. Technisches Map-Layout

Die Struktur folgt einem **spiralförmigen Aufbau** mit klar getrennten Zonen. Durch Blockaden, Schalter oder Lichtmechaniken ändern sich Routen im Verlauf.

### 5. Designprinzipien & Spielerführung

- Die Spieler werden visuell zur Mitte gelenkt
- Wiederkehrende Strukturen und Landmarken (z. B. Sonnenstatuen) erleichtern die Orientierung
- Durch Mustererkennung und Kartengedächtnis verbessert sich der Spielfluss mit der Zeit

### 6. Map-spezifische Systeme

- **Dynamische Wegführung**: Blockierungen können rotieren oder verschwinden
- **Später freischaltbare Startpunkte** erlauben gezieltere Einstiege in spätere Runs

### 7. Kartenbeispiele

- Erste Skizzen mit Spiralstruktur
- Finale Map mit Pfadverläufen, Interaktionspunkten und Blockerzonen

### 8. Zusammenhang mit Spielmechaniken

Die Karte kombiniert **Timing, Rätsel, Exploration** und sorgt dafür, dass kein Durchlauf dem anderen gleicht. Ideale Grundlage für spielmechanische Vertiefung, z. B. durch Modifikatoren oder Zeitdruck.

### 5.3 Great Depths

verwendete Asseets:

- The Depths

### Aufbau und Zielsetzung

Diese Karte legt den Fokus auf horizontale Erkundung. Der Spieler wird durch bewusst gesetzte Pfade und Plattformverbindungen durch die Umgebung geführt. Dabei sollen Neugier und Entdeckerlust durch das Leveldesign belohnt werden.

### Designmerkmale

- **Erkundungspfadstruktur:** Spieler können zwischen mehreren Pfaden wählen, was ein gewisses Maß an Entscheidungsfreiheit erlaubt.
- **Ressourcenverteilung:** Wer Erkundung betreibt, wird mit Ressourcen (z. B. Heilung, Upgrades oder Währung) belohnt.
- **Backtracking-Möglichkeiten:** Schlaufen und alternative Routen erlauben Rückkehr zu früheren Abschnitten.
- **Plattformgestaltung:** Es gibt sowohl größere Plattformen für Kampf- und Ruhebereiche als auch kleinere Übergangsplattformen.

### Bossstruktur

- **Hauptboss** auf der obersten Plattform (Endpunkt der Karte).
- **Mini-Boss** auf einer kleineren Plattform darunter als optionaler Zwischengegner.

### Kartenvariation

- **Veränderte Wege:** In dieser Variation sind einige Verbindungswege und Plattformzugänge unterbrochen oder blockiert.
- **Ziel:** Den Spieler zwingen, neue Routen zu wählen und so die Wiederholung von Durchläufen abwechslungsreicher zu gestalten.

### 5.4 Smaller Depths

verwendete Asseets:

- The Depths

### Aufbau und Zielsetzung

Diese Karte besitzt einen stärkeren vertikalen Aufbau und ist kompakter als die *Great Depths*. Die Erkundungsmöglichkeiten sind eingeschränkter, das Level fokussiert sich stärker auf Kampfintensität und vertikale Progression.

### Designmerkmale

- **Vertikale Struktur:** Der Spielfortschritt erfolgt überwiegend von oben nach unten.
- **Begrenzte Erkundung:** Es gibt nur wenige alternative Pfade.

### Gegnerdichte

- Im **unteren Bereich** konzentrieren sich vermehrt Gegnerhorden, was die Schwierigkeit steigert.
- Im **oberen Bereich** befinden sich drei kleinere Plattformen, auf denen entweder ein Boss oder ein Mini-Boss platziert werden kann.

### Kartenvariation

- **Startpunkt in der Kartenmitte:** Der Spieler beginnt in der Mitte der Karte, wobei der Zugang zum unteren Bereich blockiert ist.
- **Ziel:** Diese verkürzte Variante ermöglicht einen schnellen Bosskampf mit minimaler Erkundung.
- **Einsatz:** Ideal für Speedruns, Herausforderungsläufe oder Storyabschnitte mit Zeitdruck.

### **Lava-Kammer**

verwendete Asseets:

Volcano

### 1. Überblick / Ziel des Map Designs

Die Lava-Kammer ist eine geschlossene Boss-Arena im Inneren eines aktiven Vulkans. Ziel ist es, eine bedrohliche, feindselige Umgebung zu schaffen, die nicht nur durch den Boss, sondern auch durch die Umgebung selbst gefährlich wirkt. Der Kampf fokussiert sich auf **ständige Bewegung, geschicktes Positionieren und Kiten**.

### 2. Struktur und Layout

- **Spielbare Fläche:** Ein rechteckiges, flaches Podest inmitten eines Lavasees
- **Umgebung:** Glühende Lava umrandet die Plattform vollständig
- **Zugang:** Im Südosten über einen Lava-Brocken mit goldglühender Brücke
- **Arena-Elemente:** Vulkanische Felsen, Gesteinsbrocken, kleinere Lavaschlote zur Deckung oder als Hindernisse

### 3. Gameplay-Elemente

- **Direkter Bosskampf** – keine vorgelagerte Erkundung
- **Environmental Hazards:**
    - **Nicht-begehbare Lava** als natürliche Begrenzung
    - **Deckungsobjekte / Hindernisse** bieten Möglichkeiten zum Kiten
    - Optionale Gefahr durch **aktive Lavaschlote** oder eruptive Bodenattacken
- **Taktisches Spiel:** Bewegliche Gegner können durch gezielte Positionierung zum Fehler verleitet werden

### 4. Technisches Map-Layout

- **Klare Kampfzone:** Alles außerhalb der Plattform ist tödlich (Instant Death / Knockback-Tod)
- **Hindernisse als strategische Werkzeuge:**
    - Ermöglichen Sichtunterbrechung und kurzzeitigen Schutz
    - Einschränkung von Flächenangriffen
- (Optional): Triggerzonen für **vulkanische Eruptionen oder Lava-Säulen**, die zufällig erscheinen

### 5. Designprinzipien & Player Guidance

- **Zentrale Ausleuchtung:** Glühende Partikel, glimmender Boden, Lavaanimationen lenken den Blick auf Gefahrenzonen
- **Natürlich wirkende Begrenzung:** Spieler wissen intuitiv, wo sie sich bewegen dürfen
- Fokus auf **Skill-lastigen Kampf** ohne Fluchtmöglichkeit oder Pausen

### 6. Kartenbeispiel

**Bildreferenz:**

Die Arena besteht aus:

- Zentralem Lavaplateau mit unregelmäßigen Felsen
- Südlicher Einstiegspunkt über eine glühende Brücke
- Zahlreiche kleinere Lavaelemente und Gestein, die als taktische Zonen dienen

### 7. Zusammenhang mit Spielmechaniken

- Arena ist auf **Mobility, Timing und Raumkontrolle** ausgelegt
- Der Boss kann Fähigkeiten besitzen, die die **Bewegung des Spielers einschränken** oder **Lavaelemente aktivieren**
- (Optional): Lavaränder könnten bei bestimmten Bossphasen überfluten

### 8. Visuelles Thema / Atmosphäre

- **Feuer, Zerstörung, Endkampf-Stimmung**
- Farbkontrast zwischen dunklem Gestein und gleißend roter Lava
- Gesteinsbrocken mit glühenden Rissen verstärken das Gefühl von instabiler Gefahr
- **Dramatische Musik- und Partikeleffekte** unterstützen das Finale

### **Barbarenlager**

verwendete Asseets:

Ancient ruins

Grassland V2

### 1. Überblick / Ziel des Map Designs

Das Barbarenlager ist eine eigenständige Boss-Arena mit dem Fokus auf einen direkten, ritualisierten Einzelkampf. Ziel ist es, durch das klare Arena-Design und die visuelle Inszenierung eine barbarisch-kriegerische Atmosphäre zu vermitteln – roh, archaisch und ehrvoll.

### 2. Struktur und Layout

- **Spielbare Fläche:** Eine rechteckige, leicht erhöhte Wiese, umrundet von einer steinernen Einfassung
- **Kampfbereich:** Flach, offen, ohne Hindernisse – ideal für einen frontalen Kampf
- **Zentrale Landmarke:** Ein massives Schwert als Monument, flankiert von gebogenen Knochen – darunter ein Thron mit Trophäen
- **Dekorative Randbereiche:** Zeltlager, Knochenüberreste, Wächterstatuen, Bäume und Banner, die das Territorium eines Barbarenstammes inszenieren
- **Einstiegspunkt:** Im Süden (Treppenaufgang) – Spieler betritt die Arena feierlich und frontal

### 3. Gameplay-Elemente

- **Sofortiger Bosskampf** – keine Erkundung, keine Fluchtwege
- Fokus auf **Bewegung, Timing und Ausweichen**
- Keine Deckung: Die Kampfmechanik belohnt präzises Positionieren
- Optionale Erweiterung: Zuschauende NPCs am Rand, die auf Bossphasen oder Treffer reagieren

### 4. Technisches Map-Layout

- **Ein klarer, rechteckiger Kampfbereich** mit definierter Spielfeldgrenze
- Kampfarena vollständig sichtbar – keine Kamerabarrieren
- **Nicht begehbare Kulissen** (Bäume, Zelte, Skelett eines Riesentiers) verstärken die Kulisse ohne das Gameplay zu stören

### 5. Designprinzipien & Player Guidance

- **Zentrale visuelle Dominanz:** Das Großschwert und der Thron lenken den Blick
- **Symmetrie und klare Wegeführung** unterstützen das „rituelle Kampf“-Gefühl
- Die offene Fläche erlaubt volle Übersicht über Bossmuster

### 6. Map-spezifische Systeme

- Keine Erkundung, kein Backtracking
- Bosskampf startet automatisch bei Betreten
- (Optional) Triggerzonen für Zuschauer-Animationen, Throninteraktion oder Kampfbeginn

### 7. Kartenbeispiel

**Bildreferenz:**

Die Arena besteht aus:

- Zentralem Kampfplatz mit Schwert-Statue und Knochen
- Linke Seite: Schädel eines Riesen mit Stoffplane
- Rechte Seite: Barbarenzelt mit Lagerfeuer
- Nord-Süd-Achse: Inszenierter Kampfeinzug des Spielers

### 8. Zusammenhang mit Spielmechaniken

- Der Bosskampf basiert auf Mechaniken wie **Aggro-Management, Nahkampfreichweite und Ausweichverhalten**
- Kein Terrain-Schaden, keine Plattformgefahren – das Risiko liegt allein in der Boss-KI
- Klare Struktur erlaubt es, **Angriffsmuster und Phasenwechsel** deutlich erkennbar darzustellen

### 9. Visuelles Thema / Atmosphäre

- **Grasfläche mit martialischer Symbolik**: Schwerter, Schädel, Banner, Thron
- **Farbkontrast** zwischen warmem Laub und kaltem, totem Geäst schafft mystische Kriegsstimmung
- Ein starker Kontrast zur Lava-Arena: keine Hitze, aber rohe, physische Präsenz

# Sound-Design

## Überblick

Sound ist ein zentrales Gestaltungselement unseres Spiels. Er erzeugt Atmosphäre, unterstützt die Immersion und hilft den Spielern, sich in der Welt zurechtzufinden. Unser Ziel ist es, den Spielern durch gezielte Klänge und Musik das Gefühl zu vermitteln, wirklich Teil dieser Welt zu sein – vom ruhigen Basislager bis zum dramatischen Bosskampf. Jeder Sound wurde mit Bedacht gewählt und im Kontext getestet, um eine stimmige und funktionale Klangwelt zu schaffen.

---

## Atmosphäre & Klangwelten

### Die Basis

In der Heimatbasis herrscht Ruhe. Eine sanfte Hintergrundmusik vermittelt Sicherheit und gibt den Spielern Zeit zum Durchatmen. Dezente Klick- und Navigationsgeräusche im UI machen die Bedienung greifbar und geben sofort Rückmeldung auf Aktionen.

**Wirkung:**

- Gefühl von Kontrolle
- Kontrast zur Action in der Spielwelt
- Erholungspunkt zwischen Kämpfen

---

### Die Welt

Außerhalb der Basis wird die Klangkulisse lebendiger: dezente Windgeräusche, Vogelstimmen oder kristallines Flirren begleiten die Reise durch offene Gebiete. Beim Kristallabbau sorgen spezifische Mining-Geräusche für direktes Feedback und verstärken das Gefühl, mit der Welt zu interagieren.

**Wirkung:**

- Naturhaftigkeit und Weite
- Spieler spüren durch den Sound, wenn sie etwas Bedeutendes tun
- Audio-Hinweise leiten subtil durch die Spielwelt

---

### Der Kampf

Im Kampfgeschehen sind Fähigkeitensounds direkt, klar und kraftvoll gestaltet, damit Aktionen nachvollziehbar bleiben. Jeder Skill erhält ein akustisches Profil, das ihn nicht nur effektiv, sondern auch befriedigend klingen lässt.

**Wirkung:**

- Steigerung der Spannung
- Jeder Skill fühlt sich durch Sound-Feedback „mächtig“ an
- Orientierung im Chaos des Gefechts

---

### Bosskämpfe

Jede Bossarena besitzt eine eigene, dichte Klangwelt. Die Musik ist orchestral, bedrohlich und atmosphärisch intensiv. Jeder Boss hat ein unverwechselbares akustisches Motiv, das bereits beim Betreten der Arena Spannung aufbaut. Soundeffekte von Bossangriffen sind meist basslastig und raumfüllend – sie unterstreichen Größe und Bedrohung.

**Wirkung:**

- Gänsehaut-Effekt beim Arena-Betreten
- Akustische Identität für jeden Boss
- Unterstützung der Dramatik und Bedeutung

---

## UI & Feedback

Auch Menüs, Buttons und Systemmeldungen wurden bewusst vertont. Ein Klick auf einen Button löst einen dezenten „Tap“-Sound aus. So entsteht ein konsistentes, hochwertiges Interface-Erlebnis.

**Wirkung:**

- Schnelle Rückmeldung auf Aktionen
- Erhöhung der Bedienfreundlichkeit
- Klangliche Konsistenz im gesamten Spiel

---

## Klanggestaltung & Auswahl

Alle Klänge wurden so gewählt, dass sie sowohl zur Spielmechanik als auch zur gewünschten Stimmung passen. Kein Sound wirkt zufällig – jeder hat eine spezifische Funktion im Erlebnisdesign. Wiedererkennbarkeit, Einbindung in die Spielwelt und funktionale Klarheit standen im Fokus der Entwicklung.

---

## Zukunft & Erweiterbarkeit

Das Sound-System ist modular und skalierbar aufgebaut, sodass es problemlos um weitere Klanglandschaften ergänzt werden kann. Geplant sind zukünftige Erweiterungen wie:

- **Dynamischere Übergänge**, z. B. Musikwechsel je nach Tageszeit oder Spielsituation
- **Spezifische Ambients** für neue Regionen wie düstere Dungeons oder bizarre Zwischenräume
- **Mehr musikalische Tiefe** durch Layered Audio und adaptive Musik
- **Kontextuelle Sounds**, die auf Spieleraktionen und Umgebung reagieren

---

## Visualisierung & Arbeitsweise

Zur Verdeutlichung unserer Arbeit mit dem Sound-System sind folgende Beispielbilder geplant:

- Radiusanzeige eines 3D-Sounds im Editor
- Auswahl eines Sounds in der zentralen Soundverwaltung
- Beschrifteter Inspektor einer UI-Sound-Komponente
- Musik beim Kampf – Beispielkomposition aus Ableton


