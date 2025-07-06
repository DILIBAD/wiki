---
title: Service Lifecycle
description: 
published: true
date: 2025-04-18T10:44:39.811Z
tags: 
editor: markdown
dateCreated: 2025-04-18T10:44:39.811Z
---

# ♻️ Service Lifecycle (Init, Shutdown, Dependencies)

## ✨ Ziel
Diese Seite beschreibt den Lebenszyklus eines Service-Objekts, wie es im Spiel initialisiert, verwendet und wieder abgeschaltet wird. Besonderer Fokus liegt auf der Auflösung von Abhängigkeiten und optionalen Fallbacks.

---

## 💡 Lebenszyklus eines Service

1. **Registrierung**
   - Ein Service wird dem `ServiceManager` hinzugefügt (explizit oder dynamisch).

2. **Initialisierung (`Initialize`)**
   - Alle in `Dependencies` angegebenen Services werden zuerst aufgelöst und initialisiert.
   - Falls ein Pflicht-Service fehlt, wird versucht ein Fallback aus `OptionalDependencies` zu laden.
   - Danach wird `Initialize(IServiceLocator)` aufgerufen.

3. **Verwendung (Runtime)**
   - Der Service ist im Spiel per `GlobalServiceLocator.Instance.Get<T>()` verfügbar.

4. **Shutdown**
   - Wird vom `ServiceManager` explizit aufgerufen oder am Ende der Laufzeit für Cleanup.

---

## 🔗 Dependencies & OptionalDependencies

### Pflichtabhängigkeiten:
```csharp
public Type[] Dependencies => new[] { typeof(ConfigService), typeof(LoggerService) };
```
- Müssen **verfügbar und initialisiert** sein, bevor der Service starten kann.

### Optionale Abhängigkeiten / Fallbacks:
```csharp
public Type[] OptionalDependencies => new[] { typeof(DummyLoggerService) };
```
- Werden verwendet, wenn Pflicht-Services nicht gefunden werden.
- Gut geeignet für Editor-/Test-Modus oder Feature-Toggles.

---

## ✂ Beispiel-Service mit Lifecycle
```csharp
public class InventoryService : IService
{
    public string Name => "Inventory";
    public bool IsInitialized { get; private set; }

    public Type[] Dependencies => new[] { typeof(PlayerDataService) };
    public Type[] OptionalDependencies => Array.Empty<Type>();

    private PlayerDataService _playerData;

    public void Initialize(IServiceLocator locator)
    {
        _playerData = locator.Get<PlayerDataService>();
        // Lösung aus gespeicherten Daten
        IsInitialized = true;
    }

    public void Shutdown()
    {
        // Inventar zurückspeichern, Aufräumen
        IsInitialized = false;
    }
}
```

---

## 🧪 Best Practices
- Services sollten **idempotent** sein: `Initialize()` darf mehrfach ohne Seiteneffekte aufgerufen werden.
- Setze `IsInitialized`, um fehlerhafte Doppelladungen zu verhindern.
- Nutze `Shutdown()` für Cleanup bei Szenenwechsel, Tests oder Editor-Stop.
- Nutze `OptionalDependencies`, um testbare Dummies einzubinden.

---

## 📌 Siehe auch
- [Servicesystem](servicesystem-overview.md)
- [Bootstrapping & Unity Lifecycle](Bootstrapping-unity-lifecycle.md)

Letzte Aktualisierung: April 2025

