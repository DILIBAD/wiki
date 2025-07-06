## Scene (in Unity)

**Definition:**  
In Unity bezeichnet eine **Scene** eine eigenständige „Bühne“ oder Umgebung, in der dein Spiel oder deine Anwendung abläuft. Man kann sich eine Scene wie ein einzelnes Kapitel in einem Buch vorstellen: Jede Scene enthält ihre eigenen Objekte, Einstellungen und Logik.

---

### Woraus besteht eine Scene?
- **GameObjects:** Alles, was du in deiner Scene siehst oder hörst – Figuren, Gebäude, Kameras, Lichter, Tonquellen usw.  
- **Komponenten:** „Bausteine“, die den GameObjects Verhalten verleihen (z. B. ein Mesh-Renderer, damit ein Objekt sichtbar wird, oder ein Skript, das es steuert).  
- **Hierarchie:** Die Anordnung der GameObjects als Baumstruktur, ähnlich einem Datei-Explorer (z. B. ist eine Lampe ein Kind des Lampenständers).  
- **Einstellungen:** Physikparameter, Beleuchtungsvoreinstellungen oder Umgebungsgeräusche, die global für die Scene gelten.

---

### Warum braucht man mehrere Scenes?
- **Kapitelstruktur:** Wie in einem Buch jedes Kapitel eine neue Situation schildert, kann jede Unity-Scene einen anderen Level, ein Menü oder eine Zwischensequenz abbilden.  
- **Performance:** Große Welten lassen sich in kleinere Bereiche aufteilen, um Speicher und Rechenleistung effizient zu nutzen.  
- **Organisation:** Trennung von verschiedenen Spielbereichen (z. B. Hauptmenü, Spielwelt, Ladebildschirm) macht Entwicklung und Wartung übersichtlicher.

---

### Beispiele
1. **Hauptmenü-Scene:** Zeigt Buttons „Start“, „Optionen“ und „Beenden“.  
2. **Level-1-Scene:** Enthält Terrain, Gegner, Sammelobjekte und die Spielfigur.  
3. **Lade-Scene:** Eine schwarze Fläche mit Ladebalken, während Level-Daten im Hintergrund geladen werden.

---

### Metapher
> Stell dir ein Theaterstück vor:  
> - **Die Bühne (Scene)** ist der Raum, in dem das Stück gezeigt wird.  
> - **Requisiten und Kulissen (GameObjects)** sind auf der Bühne platziert.  
> - **Schauspieler und ihre Drehbücher (Komponenten/Skripte)** erwecken die Szenerie zum Leben.  
> - Wenn das nächste Kapitel beginnt, wird die Bühne umgebaut oder eine neue Bühne vorbereitet – so etwas wie ein Szenenwechsel.

---

**Kurzfassung:**  
Eine Unity-Scene ist ein in sich abgeschlossener Container für alle visuellen, auditiven und logischen Elemente eines Spielabschnitts oder einer Anwendungssituation. Mit mehreren Scenes strukturierst und optimierst du dein Projekt – genau wie verschiedene Kapitel in einem Buch oder Szenen in einem Theaterstück.```
