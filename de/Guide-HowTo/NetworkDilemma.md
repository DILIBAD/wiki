---
title: Network Dilemma
description: 
published: true
date: 2025-04-19T14:53:08.333Z
tags: 
editor: markdown
dateCreated: 2025-04-19T14:53:08.333Z
---

# 🔗 Kommunikation zwischen Client und Server mit Mirror (Unity C#)

> **TL;DR:**  
> In Mirror kommuniziert der Client über **[Command]**-Methoden mit dem Server.  
> Der Server informiert Clients mit **[ClientRpc]** oder **[TargetRpc]**.  
> Nutze das Open-Source-Projekt **[uMMORPG2D](https://github.com/MirrorNetworking/uMMORPG2D)** als Best-Practice-Beispiel.  
> 📚 Doku: [mirror-networking.gitbook.io/docs](https://mirror-networking.gitbook.io/docs)

---

## 🧱 Grundkomponenten von Mirror

| Komponente | Zweck |
| --- | --- |
| `NetworkManager` | Verwaltet Verbindungen, Spielstart/-ende, Szenenwechsel |
| `NetworkBehaviour` | Basis für alle Netzwerk-Komponenten, erweitert `MonoBehaviour` |
| `NetworkIdentity` | Muss auf jedem netzwerkfähigen GameObject sein |
| `[Command]` | Vom **Client aufgerufen**, **auf dem Server** ausgeführt |
| `[ClientRpc]` | Vom **Server aufgerufen**, **an alle Clients** gesendet |
| `[TargetRpc]` | Vom **Server an einen bestimmten Client** gesendet |

---

## 🧪 Beispiel 1 – Client sendet Befehl an Server (`[Command]`)

```csharp
public class PlayerController : NetworkBehaviour
{
    void Update()
    {
        if (isLocalPlayer && Input.GetKeyDown(KeyCode.Space))
        {
            CmdRequestJump();
        }
    }

    [Command]
    void CmdRequestJump()
    {
        // Server entscheidet über Ausführung
        GetComponent<Rigidbody2D>().AddForce(Vector2.up * 5f, ForceMode2D.Impulse);
    }
}
```

📌 **Hinweis**: Nur der **lokale Client** ruft `CmdRequestJump()` auf, die Methode wird **auf dem Server** ausgeführt.

---

## 🧪 Beispiel 2 – Server informiert alle Clients (`[ClientRpc]`)

```csharp
public class Chest : NetworkBehaviour
{
    [Server]
    public void Open()
    {
        // Logik nur auf Server
        RpcPlayOpenAnimation();
    }

    [ClientRpc]
    void RpcPlayOpenAnimation()
    {
        // Auf allen Clients: Animation starten
        GetComponent<Animator>().SetTrigger("Open");
    }
}
```

---

## 🧪 Beispiel 3 – Server antwortet gezielt einem Client (`[TargetRpc]`)

```csharp
public class DialogueManager : NetworkBehaviour
{
    [Server]
    public void TriggerDialogue(NetworkConnectionToClient conn)
    {
        TargetShowDialogue(conn, "Willkommen im Dorf!");
    }

    [TargetRpc]
    void TargetShowDialogue(NetworkConnectionToClient target, string text)
    {
        UIManager.Instance.ShowMessage(text);
    }
}
```

---

## 🧪 Beispiel 4 – Objekt über Netzwerk spawnen

```csharp
public GameObject bulletPrefab;

[Command]
void CmdFire()
{
    GameObject bullet = Instantiate(bulletPrefab, transform.position, Quaternion.identity);
    NetworkServer.Spawn(bullet); // Wichtig!
}
```

---

## 📦 Best Practices mit uMMORPG2D

👉 [uMMORPG2D](https://github.com/MirrorNetworking/uMMORPG2D) ist ein vollständiges Mirror-basiertes Projekt mit:

- Netzwerkbasiertem Kampfsystem
- Inventar und Item Sync
- NPC Interaktionen
- Bewegung und Autoritätshandling
- Sauberer Trennung von Client & Server Logik

📎 Ideal als Lernquelle oder Blaupause für eigene Systeme.

---

## 📚 Offizielle Ressourcen

- 📖 Dokumentation: [https://mirror-networking.gitbook.io/docs](https://mirror-networking.gitbook.io/docs)
- 💾 GitHub: [https://github.com/MirrorNetworking/Mirror](https://github.com/MirrorNetworking/Mirror)
- 🎓 Tutorials: [YouTube Mirror Networking Channel](https://www.youtube.com/@vis2k)
- 🔍 Referenzprojekt: [https://github.com/MirrorNetworking/uMMORPG2D](https://github.com/MirrorNetworking/uMMORPG2D)

---

## 🧼 Fazit

Mirror bietet eine **klare, robuste Struktur für Client-Server-Kommunikation** in Unity.  
Durch Commands, RPCs und Autoritätsmodelle lässt sich Gameplay sauber trennen und absichern.  
👉 Nutze uMMORPG2D als praxisnahe Referenz für eigene Netzwerk-Features.