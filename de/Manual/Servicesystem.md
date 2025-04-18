---
title: Service System
description: 
published: true
date: 2025-04-18T10:37:29.060Z
tags: 
editor: markdown
dateCreated: 2025-04-18T10:37:29.060Z
---

# 🧩 Servicesystem

## ✨ Ziel
Das Servicesystem bildet das modulare Herzstück der Anwendung. Es erlaubt, lose gekoppelte Komponenten über einen gemeinsamen ServiceManager zu registrieren, zu initialisieren und im globalen Kontext verfügbar zu machen.

---

## 🧱 Struktur

### Interfaces
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

public interface IServiceLocator
{
    T Get<T>() where T : class, IService;
    IService Get(Type type);
    bool IsAvailable(Type type);
}
```

### ServiceManager
Verwaltet Registrierung, Initialisierung und Auflösung von Services.

```csharp
public class ServiceManager : IServiceLocator
{
    private Dictionary<Type, IService> _services = new();
    private Dictionary<Type, IService> _fallbacks = new();

    public void RegisterService(IService service, bool isFallback = false)
    {
        var type = service.GetType();
        if (isFallback)
            _fallbacks[type] = service;
        else
            _services[type] = service;
    }

    public void InitializeAll()
    {
        foreach (var service in _services.Values)
            InitializeService(service);
    }

    private void InitializeService(IService service)
    {
        if (service.IsInitialized) return;

        foreach (var dep in service.Dependencies)
        {
            if (!_services.TryGetValue(dep, out var dependency))
            {
                if (_fallbacks.TryGetValue(dep, out var fallback))
                    RegisterService(fallback);

                dependency = _services[dep];
            }
            InitializeService(dependency);
        }

        service.Initialize(this);
    }

    public T Get<T>() where T : class, IService => _services[typeof(T)] as T;
    public IService Get(Type type) => _services.TryGetValue(type, out var s) ? s : null;
    public bool IsAvailable(Type type) => _services.ContainsKey(type);
}
```

### GlobalServiceLocator
Globale Zugriffsmöglichkeit auf Services (Singleton-Style).

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

## 👀 Verknüpfung zum Bootstrap
Das Servicesystem wird im `Bootstrapper` verwendet, um Services in verschiedenen Unity-Load-Phasen bereitzustellen:

- `SubsystemRegistration`: Fallbacks, Logging, Systeminfo
- `BeforeSceneLoad`: Core-Services, Headless-Prüfung, PreServer/Client
- `AfterSceneLoad`: szenenabhängige Services (UI, Spawner, World)

**Details siehe:** `Bootstrapping & Unity Lifecycle`

---

## ✂ Eigene Services schreiben
```csharp
public class MyService : IService
{
    public string Name => "MyService";
    public Type[] Dependencies => new[] { typeof(ConfigService) };
    public Type[] OptionalDependencies => new Type[0];
    public bool IsInitialized { get; private set; }

    public void Initialize(IServiceLocator locator)
    {
        var config = locator.Get<ConfigService>();
        // Initialisiere mit Config
        IsInitialized = true;
    }

    public void Shutdown()
    {
        // Cleanup
        IsInitialized = false;
    }
}
```

Dann im Bootstrap:
```csharp
serviceManager.RegisterService(new MyService());
```

---

## ℹ Hinweise
- Services müssen idempotent sein: `Initialize()` darf mehrfach sicher aufgerufen werden
- OptionalDependencies erlauben Flexibilität bei Feature-Toggles oder Testumgebungen
- Fallbacks werden verwendet, wenn echte Abhängigkeiten fehlen

---

Letzte Aktualisierung: April 2025