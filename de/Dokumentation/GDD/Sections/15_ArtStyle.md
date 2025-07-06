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



