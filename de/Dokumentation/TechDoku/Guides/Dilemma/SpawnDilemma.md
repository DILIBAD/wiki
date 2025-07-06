---
title: Spawn Dilemma
description: 
published: true
date: 2025-04-19T14:45:31.005Z
tags: 
editor: markdown
dateCreated: 2025-04-19T14:45:31.005Z
---

### Wie spawne ich Objekte in der Welt?

> **TL;DR:**  
> Spawns im Netzwerk-Kontext laufen **ausschließlich über den ServerSpawnService**.  
> Lokale Elemente wie UI oder Audio werden über den **ClientSpawnService** erzeugt.  
> Alle Systeme arbeiten gegen Interfaces, damit die konkrete Umsetzung austauschbar ist (z. B. für Pooling).

---

## 🧩 Problemstellung

In Multiplayer-Umgebungen ist es entscheidend, dass nur der Server die Kontrolle über Netzwerk-Objekte hat. Gleichzeitig soll die Spawn-Logik **zentralisiert** werden, damit alle Systeme **einheitlich** damit arbeiten können – egal ob Netzwerkobjekt oder lokales Objekt.

---

## ✅ Ziel

- **Zentraler Spawn-Zugriff** (kein Wildwuchs von `Instantiate`)
- **Trennung von Server/Client-Kontext**
- Vorbereitung für **Object Pooling**, ohne alle Systeme später ändern zu müssen
- **Deklarative Spawns** durch Konfigurationen (ScriptableObjects, Structs)

---

## 🧱 Architekturüberblick

| Ebene | Beschreibung |
| --- | --- |
| `ISpawnService` | Interface für allgemeine Spawns |
| `IServerSpawnService : ISpawnService` | Wird vom Server verwendet |
| `IClientSpawnService : ISpawnService` | Wird vom Client verwendet |
| `SpawnRequest` / `SpawnData` | Enthält was, wie, wo gespawnt werden soll |
| `ServerSystem` | Stellt Spawn-Anfrage an `IServerSpawnService` |
| `ClientSystem` | Stellt Spawn-Anfrage an `IClientSpawnService` oder sendet Anfrage an Server |

---

## 🧪 Beispiel: Vereinfachter `ServerSpawnService`

> ⚠️ **Hinweis:** Dies ist ein vereinfachtes Beispiel zur Veranschaulichung des Prinzips – **keine vollständige Produktionslösung!**

```csharp
public interface ISpawnService
{
    GameObject Spawn(SpawnRequest request);
}

public interface IServerSpawnService : ISpawnService { }

public interface IClientSpawnService : ISpawnService { }

[System.Serializable]
public struct SpawnRequest
{
    public GameObject prefab;
    public Vector3 position;
    public Quaternion rotation;
    public SpawnConfig config; // z. B. ScriptableObject oder weitere Daten
}
```

```csharp
public class ServerSpawnService : IServerSpawnService
{
    public GameObject Spawn(SpawnRequest request)
    {
        // Später austauschbar mit Pooling
        var instance = GameObject.Instantiate(request.prefab, request.position, request.rotation);

        // Setup anhand von Konfiguration
        if (request.config != null)
            request.config.ApplyTo(instance);

        // Hier würde ggf. NetworkServer.Spawn() o.Ä. folgen
        return instance;
    }
}
```

```csharp
// Beispielhafte Konfiguration – z. B. als ScriptableObject
[CreateAssetMenu(menuName = "Spawning/SpawnConfig")]
public class SpawnConfig : ScriptableObject
{
    public float initialHealth;
    public string team;

    public void ApplyTo(GameObject go)
    {
        if (go.TryGetComponent(out ISpawnableEntity entity))
        {
            entity.Setup(initialHealth, team);
        }
    }
}

public interface ISpawnableEntity
{
    void Setup(float health, string team);
}
```

---

## 📌 Anwendung im Code

### Serverseitig (z. B. bei Hit auf Spawner)

```csharp
_spawnService.Spawn(new SpawnRequest {
    prefab = enemyPrefab,
    position = spawnPoint,
    rotation = Quaternion.identity,
    config = selectedSpawnConfig
});
```

### Clientseitig (z. B. für UI, Audio, Effekte)

```csharp
_clientSpawnService.Spawn(new SpawnRequest {
    prefab = audioEffectPrefab,
    position = player.position,
    rotation = Quaternion.identity
});
```

---

## 🧼 Fazit

Die Nutzung eines zentralen `SpawnService` mit klarer Trennung zwischen Server und Client bietet:

- Einheitliche Verwaltung von Instanziierungen
- Leichte Integration von Pooling
- Möglichkeit, Spawns deklarativ über Konfigurationsobjekte zu steuern
- Sicherstellung der Kontrolle im Netzwerk (nur Server entscheidet über Netzobjekte)