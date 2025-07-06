---
title: Service System
description: 
published: true
date: 2025-04-18T10:47:44.375Z
tags: 
editor: markdown
dateCreated: 2025-04-18T10:37:29.060Z
---

# 🧩 Servicesystem

## ✨ Ziel
Das Servicesystem stellt eine flexible, modulare Infrastruktur bereit, mit der sowohl Client- als auch Serverkomponenten lose gekoppelt über gemeinsame Interfaces verwaltet und genutzt werden können. Die Grundlage bilden `IService`, `ServiceManager` und der globale `ServiceLocator`.

---

## 🧱 Architekturübersicht

### Ziele des Systems
- 💡 **Lose Kopplung** zwischen Spielsystemen
- 🔁 **Lebenszyklus-Management** (Init / Shutdown)
- 🧪 **Testbarkeit** durch optionale Abhängigkeiten und Fallbacks
- 🌐 **Zentraler Zugriff** über globalen ServiceLocator

---

## 🧩 Schnittstellen

### `IService`
```csharp
public interface IService
{
    string Name { get; }
    Type[] Dependencies { get; }
    Type[] OptionalDependencies { get; }
    bool IsInitialized { get; }

    void Initialize(IServiceLocator locator);
    void Shutdown();
}
```

### `IServiceLocator`
```csharp
public interface IServiceLocator
{
    T Get<T>() where T : class, IService;
    IService Get(Type type);
    bool IsAvailable(Type type);
}
```

---

## 🧰 ServiceManager – zentrales Management
Der `ServiceManager` verwaltet Registrierung, Initialisierung und Abhängigkeitsauflösung von Services. Dabei werden Services zunächst registriert und **gecached**, bevor sie bei Bedarf oder beim Aufruf von `InitializeAll()` initialisiert und in die aktiven Services überführt werden.

```csharp
public class ServiceManager : IServiceLocator
{
    private Dictionary<Type, IService> _activeServices = new();
    private Dictionary<Type, IService> _registeredCache = new();
    private Dictionary<Type, IService> _fallbacks = new();

    public void RegisterService(IService service, bool isFallback = false)
    {
        var type = service.GetType();
        if (isFallback)
        {
            _fallbacks[type] = service;
        }
        else
        {
            if (_activeServices.ContainsKey(type) || _registeredCache.ContainsKey(type)) return;
            _registeredCache[type] = service;
        }
    }

    public void InitializeAll()
    {
        foreach (var type in _registeredCache.Keys.ToList())
        {
            InitializeService(type);
        }
    }

    private void InitializeService(Type type)
    {
        if (_activeServices.ContainsKey(type)) return;

        if (!_registeredCache.TryGetValue(type, out var service))
        {
            if (_fallbacks.TryGetValue(type, out var fallback))
            {
                service = fallback;
                _registeredCache[type] = service;
            }
            else
            {
                throw new InvalidOperationException($"Service of type {type} not found.");
            }
        }

        foreach (var dep in service.Dependencies)
        {
            InitializeService(dep);
        }

        service.Initialize(this);
        _registeredCache.Remove(type);
        _activeServices[type] = service;
    }

    public T Get<T>() where T : class, IService => Get(typeof(T)) as T;

    public IService Get(Type type)
    {
        if (_activeServices.TryGetValue(type, out var service)) return service;
        if (_registeredCache.ContainsKey(type))
        {
            InitializeService(type);
            return _activeServices[type];
        }
        return null;
    }

    public bool IsAvailable(Type type) => _activeServices.ContainsKey(type);
}
```

---

## 🌐 GlobalServiceLocator – globaler Zugriff
```csharp
public static class GlobalServiceLocator
{
    private static IServiceLocator _instance;
    private static readonly object _lock = new();

    public static IServiceLocator Instance
    {
        get
        {
            if (_instance == null)
                throw new InvalidOperationException("GlobalServiceLocator was not initialized.");
            return _instance;
        }
    }

    public static bool IsInitialized => _instance != null;

    public static void Set(IServiceLocator locator)
    {
        lock (_lock)
        {
            if (_instance != null)
                throw new InvalidOperationException("GlobalServiceLocator is already initialized.");
            _instance = locator ?? throw new ArgumentNullException(nameof(locator));
        }
    }

    public static void Reset()
    {
        lock (_lock) { _instance = null; }
    }
}
```

---

## 🔁 Lebenszyklus der Services
Services durchlaufen folgende Phasen:
1. **Registrierung** über `RegisterService()` (Cache)
2. **Initialisierung** durch `Initialize()` – inkl. Abhängigkeitsauflösung → Übergabe in aktiven Servicebereich
3. **Laufzeitnutzung** – Zugriff über ServiceLocator oder Injection
4. **Shutdown** durch explizite Deinitialisierung

Mehr Details dazu findest du in: [Service Lifecycle](Service-Lifecycle.md)

---

## ⚙️ Integration im Bootstrapprozess
Das Servicesystem wird vom `Bootstrapper` in mehreren Unity-Phasen verwendet:

- `SubsystemRegistration`: Frühe Fallbacks & Logging
- `BeforeSceneLoad`: Core-Services, Server/Client-Kontext
- `AfterSceneLoad`: szenenbezogene Services (UI, Spawner, Game-World)

Siehe: [Bootstrapping & Unity Lifecycle](Bootstrapping-unity-lifecycle.md)

---

## ✏️ Beispiel: Eigener Service
```csharp
public class ExampleService : IService
{
    public string Name => "ExampleService";
    public Type[] Dependencies => new[] { typeof(ConfigService) };
    public Type[] OptionalDependencies => Array.Empty<Type>();
    public bool IsInitialized { get; private set; }

    public void Initialize(IServiceLocator locator)
    {
        var config = locator.Get<ConfigService>();
        IsInitialized = true;
    }

    public void Shutdown() => IsInitialized = false;
}
```

Registrierung:
```csharp
serviceManager.RegisterService(new ExampleService());
```

---

## 📌 Hinweise
- Services sollten **idempotent** sein (Mehrfach-Init sicher)
- `OptionalDependencies` erlauben alternative Implementierungen für Tests oder Feature-Toggles
- Fallbacks decken fehlende Abhängigkeiten ab
- Zugriff immer über Interfaces (`IService`, `IServiceLocator`), nie direkte Instanzen verwenden

---

Letzte Aktualisierung: April 2025

