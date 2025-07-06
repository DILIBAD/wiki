# UniTask

## Was ist UniTask?

UniTask ist ein leichtgewichtiges, asynchrones Task-System für Unity, das moderne C#-`async/await`-Patterns nutzt und als performanter Ersatz für Unity-Coroutinen (`IEnumerator`) dient.

---

## Warum UniTask statt Coroutinen?

- **Performance**  
    Coroutinen erzeugen bei jedem `yield return` neue Objekte und belasten den Garbage Collector. UniTask arbeitet mit einem internen Pool und minimiert dadurch Speicherallokationen.
    
- **Lesbarkeit**  
    Code mit `async/await` liest sich flacher und geradliniger als verschachtelte `IEnumerator`-Methoden.
    
- **Erweiterte Features**  
    UniTask bietet out-of-the-box Support für:
    
    - `CancellationToken`
        
    - Parallelisierung mit `WhenAll`/`WhenAny`
        
    - Timeouts und Retry-Mechanismen
        

---

## Technische Grundlagen

1. **Async/Await statt IEnumerator**
    
    ```csharp
    // Coroutine-Ansatz
    IEnumerator MyCoroutine()
    {
        // … Code …
        yield return new WaitForSeconds(1f);
        // … Code nach 1 Sekunde …
    }
    
    // UniTask-Ansatz
    async UniTask MyAsyncMethod()
    {
        // … Code …
        await UniTask.Delay(1000);
        // … Code nach 1 Sekunde …
    }
    ```
    
2. **Single-Threaded Scheduler**  
    UniTask erweitert den Unity-`PlayerLoop`, um alle Tasks auf dem Main Thread auszuführen. So bleibt der Zugriff auf Unity-APIs sicher.
    
3. **Zero-Allocation durch Pooling**  
    Häufig verwendete Tasks (z. B. Delay, NextFrame) werden über einen Objekt-Pool verwaltet und wiederverwendet.
    

---

## Funktionsweise im Detail

- **Task-Pools**  
    Delay- und Frame-Tasks werden aus einem Pool entnommen und nach Ausführung zurückgegeben.
    
- **Extension Methods**
    
    ```csharp
    // Coroutine ➔ UniTask
    IEnumerator SomeCoroutine() { … }
    var task = SomeCoroutine().ToUniTask();
    
    // UniTask ➔ Coroutine
    async UniTask DoWork() { … }
    var coroutine = DoWork().ToCoroutine();
    ```
    
- **Cancellation**
    
    ```csharp
    var cts = new CancellationTokenSource();
    await UniTask.Delay(5000, cancellationToken: cts.Token);
    // cts.Cancel(); kann aufgerufen werden, um den Task vorzeitig abzubrechen
    ```
    
- **Parallelisierung**
    
    ```csharp
    // Parallel ausführen und auf alle warten
    await UniTask.WhenAll(TaskA(), TaskB());
    
    // Auf den zuerst abgeschlossenen Task warten
    var (index, result) = await UniTask.WhenAny(TaskA(), TaskB());
    ```
    

---

## Beispiele & Metapher

### Einfache Verzögerung

```csharp
using Cysharp.Threading.Tasks;

async UniTask Start()
{
    Debug.Log("Starte Verzögerung…");
    await UniTask.Delay(2000);
    Debug.Log("2 Sekunden später");
}
```

### Metapher: Paket und Bote

- **Mit Coroutinen**  
    Du schickst einen Boten los und prüfst alle paar Sekunden manuell, ob das Paket angekommen ist (`yield return new WaitForSeconds(5);`).
    
- **Mit UniTask**  
    Du gibst dem Boten eine Stoppuhr (Delay-Task). Er schläft intern und weckt dich automatisch zur richtigen Zeit – ohne ständiges Nachschauen.
    

---

## Zusammenfassung

UniTask ersetzt Unity-Coroutinen durch ein modernes `async/await`-System mit:

- **Geringem Overhead** (Zero-Allocation, Pooling)
    
- **Besserer Lesbarkeit** (flacherer Code)
    
- **Vielseitigen Patterns** (Cancellation, Parallelität, Timeouts)
    

So lassen sich asynchrone Abläufe in Unity effizienter und eleganter gestalten.