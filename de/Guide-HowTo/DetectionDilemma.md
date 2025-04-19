---
title: Detection Dilemma
description: 
published: true
date: 2025-04-19T14:03:52.736Z
tags: 
editor: markdown
dateCreated: 2025-04-19T14:03:52.736Z
---

# Unity 2D Sensorik: Trigger-Events vs. Tick-basierte Sensoren

> **TL;DR:**  
> Verwende `OnTriggerEnter2D` / `OnTriggerExit2D`, wenn eine **sofortige Reaktion** erforderlich ist (z. B. Betreten einer Zone, Item Pickup).  
> Nutze `Physics2D.Overlap*` mit Tickrate, wenn es auf **regelmäßige Abfragen** ankommt oder du das Setup vollständig im Code kontrollieren möchtest.

---

## 🧩 Problemstellung

Unitys Trigger-Events (`OnTriggerEnter2D` / `OnTriggerExit2D`) sind einfach in der Verwendung, erfordern jedoch ein korrektes physikalisches Setup im Editor – z. B. einen aktivierten Trigger-Collider und ein passendes Rigidbody2D-Setup. Fehler in der Konfiguration führen oft zu schwer nachvollziehbarem Verhalten oder gar nicht ausgelösten Events.

In vielen Fällen ist eine **regelmäßige, tick-basierte Abfrage über Code** nicht nur ausreichend, sondern auch robuster und transparenter in der Handhabung.

---

## ⚖️ Vorteile beider Varianten

### ✅ Trigger-Events

- **Pro:**
  
  - Reaktion erfolgt **sofort beim Eintritt/Austritt**.
  - Einfach zu implementieren für punktuelle Interaktionen.
- **Kontra:**
  
  - Setup im Editor zwingend nötig.
  - Fehleranfällig bei vergessenen Rigidbody2D-Komponenten.
  - Kein zentralisiertes Tick-Management.

### ✅ Tick-basierte Sensoren (`Physics2D.Overlap*`)

- **Pro:**
  
  - Vollständig über Code steuerbar.
  - **Zentrale Kontrolle über Tickrate und Performance**.
  - Kein Setup im Editor nötig.
  - Gut für kontinuierliche Bereichserkennung (KI, Sensorfelder).
- **Kontra:**
  
  - Kein „sofortiges Feedback“ – minimale Latenz durch Tickrate.
  - Muss manuell gepflegt werden.

---

## 🧭 Wann sollte was verwendet werden?

| Kriterium | Trigger-Events | Tick-basierter Sensor |
| --- | --- | --- |
| Sofortige Reaktion nötig? | ✅ Ja | ❌ Nein |
| Setup im Editor gewünscht? | ⚠️ Ja, wenn es anders nicht geht oder zu komplex ist | ❌ Nein |
| Bereichserkennung / Zone / KI | ⚠️ Möglich | ✅ Optimal |
| Gefahr von Setup-Fehlern | ❌ Hoch | ✅ Gering |
| Nur einmalige Reaktion (z. B. Knopf) | ❌ Nein | ✅ Ja |

---

## 🧪 Code-Beispiele

### 1. Trigger mit generischer `List<T>`-Sammlung

```csharp
public class TriggerCollector<T> : MonoBehaviour where T : Component
{
    public List<T> Entities = new();

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.TryGetComponent(out T component))
        {
            Entities.Add(component);
        }
    }

    private void OnTriggerExit2D(Collider2D other)
    {
        if (other.TryGetComponent(out T component))
        {
            Entities.Remove(component);
        }
    }
}
```

---

### 2. Trigger-Logik ohne Liste, mit abstrakter Reaktion

```csharp
public abstract class TriggerProcessor<T> : MonoBehaviour where T : Component
{
    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.TryGetComponent(out T entity))
        {
            OnEnter(entity);
        }
    }

    private void OnTriggerExit2D(Collider2D other)
    {
        if (other.TryGetComponent(out T entity))
        {
            OnExit(entity);
        }
    }

    protected abstract void OnEnter(T entity);
    protected abstract void OnExit(T entity);
}
```

---

### 3. Tick-basierter Sensor mit `Physics2D.OverlapBoxNonAlloc`

```csharp
public abstract class TickBasedSensor<T> : MonoBehaviour where T : Component
{
    [SerializeField] private Vector2 boxSize = Vector2.one;
    [SerializeField] private Vector2 offset = Vector2.zero;
    [SerializeField] private float tickRate = 0.5f;
    [SerializeField] private LayerMask layerMask;

    private readonly Collider2D[] _results = new Collider2D[32];
    private float _timer;

    private void Update()
    {
        _timer += Time.deltaTime;
        if (_timer >= tickRate)
        {
            _timer = 0f;
            RunSensor();
        }
    }

    private void RunSensor()
    {
        Vector2 center = (Vector2)transform.position + offset;
        int count = Physics2D.OverlapBoxNonAlloc(center, boxSize, 0f, _results, layerMask);

        for (int i = 0; i < count; i++)
        {
            if (_results[i].TryGetComponent(out T component))
            {
                Process(component);
            }
        }
    }

    protected abstract void Process(T entity);
}
```

---

### 4. Multi-Sensor-Manager mit konfigurierbaren Sensoren

```csharp
public class SensorManager : MonoBehaviour
{
    [System.Serializable]
    public class SensorConfig
    {
        public string name;
        public Vector2 centerOffset;
        public Vector2 boxSize = Vector2.one;
        public float tickRate = 1.0f;
        public LayerMask layerMask;
    }

    public List<SensorConfig> sensors = new();
    private readonly Dictionary<string, float> _timers = new();

    private void Update()
    {
        foreach (var config in sensors)
        {
            if (!_timers.ContainsKey(config.name))
                _timers[config.name] = 0f;

            _timers[config.name] += Time.deltaTime;
            if (_timers[config.name] >= config.tickRate)
            {
                _timers[config.name] = 0f;
                RunSensor(config);
            }
        }
    }

    private void RunSensor(SensorConfig config)
    {
        var center = (Vector2)transform.position + config.centerOffset;
        var results = new Collider2D[32];
        int count = Physics2D.OverlapBoxNonAlloc(center, config.boxSize, 0f, results, config.layerMask);

        for (int i = 0; i < count; i++)
        {
            if (results[i].TryGetComponent(out ISensorTarget target))
            {
                target.OnSensorPing(config.name);
            }
        }
    }
}

public interface ISensorTarget
{
    void OnSensorPing(string sensorName);
}
```

---

## 🧼 Fazit

Beide Ansätze haben ihre Daseinsberechtigung – die Wahl hängt von den Anforderungen an **Timing**, **Kontrolle** und **Komplexität** ab. Trigger-Events sind reaktiv, Tick-basierte Sensoren sind kontrolliert und erweiterbar. In vielen professionellen Projekten empfiehlt sich eine Kombination aus beiden – je nach Use Case.