---
title: Bootstrapping & Unity Lifecycle
description: 
published: true
date: 2025-04-18T10:38:46.155Z
tags: 
editor: markdown
dateCreated: 2025-04-18T10:38:46.155Z
---

## ✨ Ziel
Diese Seite beschreibt, wie das Spiel beim Start systematisch initialisiert wird und wie Unitys Lifecycle-Phasen verwendet werden, um Services zu laden. Ziel ist ein stabiler, kontextabhängiger Start für Server und Client.

---

## 📅 Unity Load-Phasen
Unity bietet verschiedene Einstiegszeitpunkte für die Initialisierung:

| Phase                    | Zeitpunkt                         | Einsatzzweck |
|-------------------------|-----------------------------------|--------------|
| `SubsystemRegistration` | Ganz früh beim Engine-Start       | Logging, Fallbacks, Plattformcheck |
| `BeforeSceneLoad`       | Bevor die erste Szene geladen wird | Core-Services, Headless-Erkennung, ServiceManager-Verknüpfung |
| `AfterSceneLoad`        | Nach Laden der ersten Szene       | Szeneabhängige Services: UI, Spawner, MonoBehaviour-abhängige Systeme |

---

## 📄 Ablaufdiagramm (Text)
```
[SubsystemRegistration]
  ↓
  🌅 Frühstufe:
    • Logging
    • Fallback-Services registrieren
    • Plattform-Check (Headless / Server)
    • ServiceManager vorbereiten

[BeforeSceneLoad]
  ↓
  🔧 Vor Szene:
    • Core Services registrieren
    • Headless-Erkennung
      - Server: PreServerServices + Initialisierung
      - Client: ClientServices + Initialisierung
    • GlobalServiceLocator setzen

[AfterSceneLoad]
  ↓
  🎮 Nach Szene:
    • Zugriff auf Szeneobjekte (MonoBehaviours)
    • PostServerServices (Spawner, MatchController)
    • UI-bezogene ClientServices
```

---

## 📈 Beispielimplementierung
```csharp
public static class Bootstrapper
{
    private static bool _coreInitialized = false;
    private static bool _mainInitialized = false;
    private static bool _sceneInitialized = false;
    private static ServiceManager _serviceManager;

    [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.SubsystemRegistration)]
    private static void OnSubsystemRegistration()
    {
        if (_coreInitialized) return;
        _coreInitialized = true;
        _serviceManager = new ServiceManager();
        RegisterFallbackServices(_serviceManager);
    }

    [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]
    private static void OnBeforeSceneLoad()
    {
        if (_mainInitialized) return;
        _mainInitialized = true;

        GlobalServiceLocator.Set(_serviceManager);
        RegisterCoreServices(_serviceManager);

        if (IsHeadless())
        {
            RegisterPreServerServices(_serviceManager);
            _serviceManager.InitializeAll();
            StartServer();
        }
        else
        {
            RegisterClientServices(_serviceManager);
            _serviceManager.InitializeAll();
        }
    }

    [RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.AfterSceneLoad)]
    private static void OnAfterSceneLoad()
    {
        if (_sceneInitialized) return;
        _sceneInitialized = true;

        if (IsHeadless())
            RegisterPostServerServices(_serviceManager);
        else
            RegisterSceneClientServices(_serviceManager);

        _serviceManager.InitializeAll();
    }

    private static bool IsHeadless()
    {
#if UNITY_SERVER
        return true;
#else
        return SystemInfo.graphicsDeviceType == UnityEngine.Rendering.GraphicsDeviceType.Null;
#endif
    }
}
```

---

## 🤝 Verbindung zum Servicesystem
Das Bootstrapping nutzt den `ServiceManager` aus dem Servicesystem, registriert und initialisiert Services je nach Umgebung und Unity-Phase.

- `SubsystemRegistration`: Fallbacks & Logging vorbereiten
- `BeforeSceneLoad`: zentrale Services & Kontextentscheidungen treffen
- `AfterSceneLoad`: szenenbezogene Services initialisieren

Mehr Infos zum Service-Konzept: [Servicesystem](servicesystem-overview.md)

---

Letzte Aktualisierung: April 2025

