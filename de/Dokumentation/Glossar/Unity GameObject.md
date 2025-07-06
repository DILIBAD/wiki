## Unity NavMesh

**Definition**  
Ein **NavMesh** (Navigation Mesh) in Unity ist ein unsichtbares Netz, das über den begehbaren Flächen einer Spielszene liegt. Es dient dazu, KI-gesteuerten Charakteren (Agents) zu zeigen, wo sie sich bewegen dürfen, und ermöglicht ihnen, optimale Wege um Hindernisse herum zu finden.

---

### Kernkonzepte

1. **Backen (Bake)**  
   - Im Editor wird durch den „Bake“-Prozess das NavMesh erzeugt: Unity analysiert alle als begehbar markierten Flächen (z. B. Boden, Rampen) und erstellt daraus ein Gitternetz.  
   - Ergebnis ist ein unsichtbares Mesh, das als Grundlage für die Pfadfindung dient.

2. **NavMeshAgent**  
   - Komponente, die du an KI-Charaktere oder NPCs anhängst.  
   - Liest das NavMesh, berechnet Routen und bewegt das Objekt automatisch entlang dieser Wege, einschließlich Kollisionsvermeidung.

3. **NavMeshObstacle**  
   - Markiert bewegliche oder feste Hindernisse (z. B. Fahrzeuge, Türen, Kisten).  
   - Das System berücksichtigt diese beim Pfadfinden und umgeht aktive Hindernisse zuverlässig.

4. **NavMeshSurface**  
   - Ab Unity 2018 können mehrere Flächen („Surfaces“) in komplexen oder modularen Szenen einzeln gebacken und zur Laufzeit aktualisiert werden.  
   - Ermöglicht dynamische NavMesh-Erzeugung bei nachträglich geladenen Level-Teilen.

---

### So funktioniert’s

1. **Szenenvorbereitung**  
   - Markiere alle begehbaren Flächen mit dem Layer **Walkable**.  
   - Weise allen Hindernissen (Wände, Säulen) entweder den Layer **Not Walkable** zu oder nutze die Komponente **NavMeshObstacle**.

2. **NavMesh backen**  
   - Menü: **Window → AI → Navigation**  
   - Reiter **Bake**: Klicke auf **Bake**, um das Mesh zu erzeugen.

3. **Agent einrichten**  
   - Füge dem Charakter eine **NavMeshAgent**-Komponente hinzu.  
   - Passe Parameter an (Geschwindigkeit, Wendekreis, Abstand zu Hindernissen).

4. **Pfad verwenden**  
   - Im Skript:
     ```csharp
     using UnityEngine;
     using UnityEngine.AI;

     public class GegnerKI : MonoBehaviour {
         public Transform ziel;
         private NavMeshAgent agent;

         void Start() {
             agent = GetComponent<NavMeshAgent>();
         }

         void Update() {
             if (ziel != null) {
                 agent.SetDestination(ziel.position);
             }
         }
     }
     ```
   - Der Agent berechnet automatisch den besten Weg auf Basis des NavMesh und bewegt sich dorthin.

---

### Beispiel

Stell dir eine Bibliothek vor: Die Gänge zwischen den Regalen sind begehbar, die Regale selbst sind Hindernisse. Du „backst“ ein NavMesh, das nur die Gänge abdeckt. Deine KI-Besucher (NavMeshAgents) navigieren so geschickt zu jedem Regal, ohne durch die Bücherregale zu laufen.

---

### Metapher

> **Ein NavMesh ist wie ein unsichtbares Straßennetz in deiner Spielwelt.**  
> Alle begehbaren Flächen sind Straßen, Hindernisse die Bürgersteige oder Bauzäune. Die NavMeshAgents sind Autos mit Navigationssystem: Sie kennen das Straßennetz, berechnen die schnellste Route zum Ziel und umfahren gleichzeitig Staus (NavMeshObstacles).

---

### Anwendungsfälle

- **Gegnerverfolgung** in Action- oder Stealth-Spielen  
- **Patrouillenrouten** für Wachen in offenen Welten  
- **Bewegung von NPCs** in Städten, Märkten oder Dungeons  
- **Dynamische Aktualisierung** von NavMeshes bei veränderlichen Umgebungen

---

Mit Unity NavMesh erhältst du ein zuverlässiges, effizientes Pfadfindungssystem, das deine KI-Agenten intelligent und realistisch durch die Spielwelt führt – ohne aufwendige Eigenimplementierungen.
