
# Broker Chain Pattern

**Was ist das?**  
Ein Entwurfsmuster, bei dem eine Anfrage durch eine Kette von Bearbeitern („Handlern“) läuft, bis einer sie verarbeitet. Jeder Handler kann die Anfrage abarbeiten oder weiterreichen.

**Beispiel in Unity C#**

```csharp
public abstract class Handler
{
    protected Handler _next;
    public Handler SetNext(Handler next) { _next = next; return next; }
    public abstract void Handle(string request);
}

public class InputHandler : Handler
{
    public override void Handle(string request)
    {
        if (request == "Input")
            Debug.Log("Input verarbeitet");
        else
            _next?.Handle(request);
    }
}

public class NetworkHandler : Handler
{
    public override void Handle(string request)
    {
        if (request == "Netzwerk")
            Debug.Log("Netzwerk verarbeitet");
        else
            _next?.Handle(request);
    }
}

// Aufbau der Kette
var input = new InputHandler();
var network = new NetworkHandler();
input.SetNext(network);

// Nutzung
input.Handle("Netzwerk");  // → „Netzwerk verarbeitet“
```

**Metapher:**  
Wie eine Poststelle mit mehreren Abteilungen: Briefe werden nacheinander von Abteilung zu Abteilung weitergeleitet, bis die richtige sie bearbeitet.
