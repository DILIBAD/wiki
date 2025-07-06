# Unity Grid Component

## Definition  
Die **Unity Grid Component** ist eine grundlegende Komponente im Unity-Editor, die ein unsichtbares Gitternetz (Grid) in der Szene erzeugt. Dieses Gitternetz definiert Form, Größe und Anordnung von Zellen, auf denen sich beispielsweise Tilemaps, 2D-Sprites oder andere GameObjects leicht ausrichten lassen.

---

## Erklärung  
- **Zellen und Layout**  
  Jede Zelle im Grid hat eine feste Breite und Höhe (z. B. 1×1 Einheit). Standardmäßig ist das Layout rechteckig (Orthogonal), es sind aber auch hexagonale oder isometrische Grids möglich.  
- **Zweck**  
  Das Grid dient in erster Linie dazu, Objekte präzise zu platzieren und zu bewegen – zum Beispiel in 2D-Spielen mit Kachelkarten (Tilemaps) oder schachbrettartigen Spielfeldern.  
- **Eigenschaften**  
  - **Cell Size**: Größe einer Zelle (X, Y, Z)  
  - **Cell Layout**: Form der Zelle (Rectangle, Hexagon, Isometric)  
  - **Cell Swizzle**: Vertauschung der Achsen, um z. B. 2D-Layouts auf der X-Y-Ebene oder X-Z-Ebene zu verwenden  

---

## Beispiel  
Stell dir vor, du gestaltest ein 2D-Rollenspiel mit einer Karte aus quadratischen Kacheln (Tiles):  
1. Füge im Unity-Editor ein leeres GameObject hinzu und vergib den Namen **„Grid“**.  
2. Hänge an dieses GameObject die **Grid Component**.  
3. Lege die **Cell Size** auf `1, 1, 0` fest (jedes Tile ist 1 Einheit breit und hoch).  
4. Erzeuge ein Tilemap-Objekt als Kind des Grids – nun kannst du im Tilemap-Editor Kacheln exakt auf jede Zelle „einstecken“.  

---

## Metapher  
Die Unity Grid Component ist wie **Millimeterpapier** für deine Spielwelt:  
> Genau wie man auf Millimeterpapier Kästchen benutzt, um Zeichnungen sauber auszurichten, legt die Grid Component unsichtbare Kästchen über deine Szene. So „rasten“ Tiles und Objekte genau ein, ohne ungewollt zu verrutschen.

---  
