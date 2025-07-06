## Unity Tilemap

**Kategorie:** Spieleentwicklung / 2D-Toolkit

---

### Was ist eine Tilemap?

Eine **Tilemap** in Unity ist ein Raster aus kleinen, quadratischen „Kacheln“ (Tiles), die zusammen eine größere 2D-Spielwelt bilden. Statt jede Grafik einzeln zu platzieren, ordnet man vorgefertigte Kacheln in einem Gitter (Grid) an – ähnlich wie bei einem Mosaik.

---

### Erklärung für Fachfremde

Stell dir ein **Schachbrett** vor. Jedes der 64 Felder ist eine rechteckige Kachel. Legst du verschiedene Kacheln (z. B. Gras, Wasser, Straße) übereinander, entsteht auf einfache Weise eine gesamte Landschaft. In Unity – einer beliebten Spiel-Engine – hilft dir das Tilemap-System dabei, solche Kachel-Raster zu erzeugen, zu bearbeiten und zu animieren, ohne jede Kachel manuell verschieben oder duplizieren zu müssen.

---

### Warum Tilemaps verwenden?

- **Effizienz**  
  Große Karten aus einzelnen Bildern aufzubauen ist aufwändig. Tilemaps sparen Zeit und Ressourcen, weil viele Kacheln automatisch wiederverwendet werden.
- **Flexibilität**  
  Du kannst schnell das Aussehen ändern: Eine Gras-Kachel gegen eine Sand-Kachel austauschen, und alle betreffenden Stellen aktualisieren sich automatisch.
- **Kollisions-Management**  
  Unity erlaubt dir, pro Kachel Kollisionseigenschaften festzulegen (z. B. begehbar oder undurchdringlich).

---

### Metapher

> Eine **Tilemap** ist wie ein Lego-Set:  
> - Jede Lego-Stein-Form ist eine Tile.  
> - Das Steckbrett ist das Grid.  
> - Mit den Steinen baust du Häuser, Straßen und Landschaften.  
> - Tauscht du einen Steintyp aus, verändert sich sofort das Gesamtbild.

---

### Beispiel: Einfache Boden-Tilemap erstellen

```csharp
using UnityEngine;
using UnityEngine.Tilemaps;

public class TilemapBeispiel : MonoBehaviour
{
    public Tilemap tilemap;          // Referenz auf das Tilemap-Objekt
    public TileBase grasKachel;      // Deine Gras-Kachel aus dem Assets-Ordner
    public TileBase wasserKachel;    // Deine Wasser-Kachel aus dem Assets-Ordner

    void Start()
    {
        // Füllt eine 10×5-Fläche mit Gras
        for (int x = 0; x < 10; x++)
        {
            for (int y = 0; y < 5; y++)
            {
                tilemap.SetTile(new Vector3Int(x, y, 0), grasKachel);
            }
        }

        // Zeichnet einen Wassergraben in der Mitte
        for (int x = 0; x < 10; x++)
        {
            tilemap.SetTile(new Vector3Int(x, 2, 0), wasserKachel);
        }
    }
}
