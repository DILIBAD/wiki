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
