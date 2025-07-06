
# Singleton Pattern

**Was ist das?**  
Ein Entwurfsmuster, das dafür sorgt, dass von einer Klasse nur eine einzige Instanz existiert und global zugänglich ist.

**Beispiel in Unity C#**

```csharp
public class GameManager : MonoBehaviour
{
    private static GameManager _instance;
    public static GameManager Instance
    {
        get
        {
            if (_instance == null)
            {
                // Neue Instanz im Szenenbaum erzeugen
                var go = new GameObject("GameManager");
                _instance = go.AddComponent<GameManager>();
                DontDestroyOnLoad(go);
            }
            return _instance;
        }
    }

    void Awake()
    {
        // Überschüssige Instanzen zerstören
        if (_instance != null && _instance != this)
            Destroy(gameObject);
        else
            _instance = this;
    }

    // Weitere Methoden und Felder…
}
```

**Metapher:**  
Stell dir den Singleton wie den einzigen Schlüssel zu einem Tresor vor: Es gibt immer genau einen Generalschlüssel, der überall hin mitgenommen wird und von allen genutzt werden kann.
