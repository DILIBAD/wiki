# Blackboard Pattern

**Was ist das?**  
Ein Entwurfsmuster, bei dem verschiedene unabhängige Komponenten („Agenten“) Informationen auf einer gemeinsamen Datenstruktur (dem „Blackboard“) ablegen und von dort wieder auslesen.

- **Blackboard:** Gemeinsamer Datenspeicher
    
- **Agenten:** Module, die Daten lesen/schreiben und aufeinander reagieren
    

**Beispiel in Unity C#:**

```csharp
public static class Blackboard
{
    private static Dictionary<string, object> _data = new Dictionary<string, object>();

    public static void Set<T>(string key, T value)
    {
        _data[key] = value;
    }

    public static T Get<T>(string key)
    {
        if (_data.TryGetValue(key, out var val) && val is T t)
            return t;
        return default;
    }
}

// Agent A schreibt
Blackboard.Set("PlayerHealth", 100);

// Agent B liest
int health = Blackboard.Get<int>("PlayerHealth");
```

**Metapher:**  
Wie eine große Tafel im Lehrerzimmer, auf der Lehrkräfte Notizen, Termine oder wichtige Infos für alle hinterlassen und alle diese Infos lesen und ergänzen können.
