## Prefab

**Definition**  
Ein **Prefab** (Kurzform von *„prefabricated“*) ist in Unity eine vordefinierte Vorlage für ein oder mehrere GameObjects mitsamt all ihrer Komponenten, Einstellungen und Kind-Objekte.  

---

**Beschreibung**  
- Ein Prefab speichert den kompletten Aufbau eines GameObjects (z. B. ein 3D-Modell, dessen Kollisionsbox, Skripte und weitere Einstellungen) an einer zentralen Stelle im Projektordner.  
- Änderungen am Prefab-Asset werden automatisch auf alle Instanzen im Spiel übertragen.  
- Instanzen eines Prefabs können einzeln überschrieben („gehostet“) werden, ohne das ursprüngliche Template zu verändern.  

---

**Beispiel**  
Stell dir vor, du baust ein Jump’n’Run-Spiel und brauchst 100 Münzen:  
1. Du erstellst eine Münze mit Mesh, Collider und Sammel-Skript.  
2. Ziehst dieses GameObject in den Ordner **Assets/Prefabs** – damit entsteht das Prefab „Münze“.  
3. Ziehst nun die Münze-Prefab-Instanz 100-mal in deine Szene.  
4. Ändere später z. B. die Farbe oder das Sammel-Skript der Münze im Prefab – alle 100 Münzen in deiner Szene übernehmen die Änderung automatisch.  

---

**Metapher**  
Ein Prefab ist wie ein Ausstechförmchen in der Küche:  
- Das Förmchen (Prefab-Asset) selbst ist die Vorlage.  
- Jeder Teig-Keks (Prefab-Instanz) wird damit ausgestochen.  
- Verändert man das Förmchen, bekommen alle nachfolgenden Kekse die neue Form – bereits ausgestochene Kekse muss man jedoch einzeln nachbearbeiten, falls man die Änderung rückwirkend übernehmen möchte.  
