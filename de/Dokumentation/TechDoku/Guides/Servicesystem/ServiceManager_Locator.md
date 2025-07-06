---
title:  ServiceManager & Locator
description: 
published: true
date: 2025-04-18T10:44:01.188Z
tags: 
editor: markdown
dateCreated: 2025-04-18T10:44:01.188Z
---

# 🧭 ServiceManager & Locator (Konzept + Interface)

## 🎯 Ziel
Dieses Dokument beschreibt das Konzept und die Architektur des `ServiceManager` sowie des `IServiceLocator`. Sie bilden das Rückgrat der modulbasierten Struktur des Spiels und ermöglichen lose Kopplung zwischen Systemen.

---

## 🧩 Konzept

### Warum ein ServiceManager?
- Vermeidung harter Abhängigkeiten zwischen Komponenten
- Zentrale Verwaltung von Lebenszyklen (Init/Shutdown)
- Dynamische Fallback-Strategien und Modularisierung
- Vereinfachter Zugriff über einen globalen Locator

### Grundidee
- Services implementieren das Interface `IService`
- Sie registrieren sich beim `ServiceManager`
- Zugriff erfolgt über den `IServiceLocator` (z. B. global über `GlobalServiceLocator`)

---

## 🧱 Interfaces

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

## 🧰 ServiceManager
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

---

## 🌐 GlobalServiceLocator
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

## 📎 Vorteile
- Services können in beliebiger Reihenfolge registriert werden
- Initialisierungsreihenfolge wird automatisch durch Abhängigkeiten bestimmt
- Möglichkeit zur Verwendung alternativer Implementierungen über Fallbacks
- Einheitlicher Zugriff via Interface statt direkter Referenz

---

## 🔗 Verwandte Themen
- [Service Lifecycle (Init, Shutdown, Dependencies)](Service-Lifecycle.md)
- [Bootstrapping & Unity Lifecycle](Bootstrapping-unity-lifecycle.md)

Letzte Aktualisierung: April 2025

