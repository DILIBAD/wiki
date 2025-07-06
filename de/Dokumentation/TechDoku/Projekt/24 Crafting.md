# Crafting System

# TLDR

Das Crafting-System verwaltet alle Rezeptvorlagen und synchronisiert für jeden Spieler bekannt­gegebene Rezepte über das Netzwerk. Es ermöglicht die Prüfung benötigter Ressourcen und das Ausführen von Craft­-Aufträgen sowohl auf Client- als auch auf Serverseite, wobei Erweiterungen für Freischalt­mechaniken einfach integriert werden können.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um im Multiplayer-Umfeld mit Mirror Networking und MST Master Server Toolkit:

- eine zentrale Verwaltung aller Crafting-Rezepte zu bieten,
    
- die Synchronisation bekannter Rezepte zwischen Server und Clients sicherzustellen,
    
- die Ressourcenprüfung und den eigentlichen Craft­vorgang separiert auf Client und Server abzubilden.
    

Es löst das Problem der dezentralen Rezeptverwaltung und garantiert Konsistenz von Spiel­zustand und Inventar in verteilten Umgebungen.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **RecipeTemplate** (ScriptableObject) als zentrale Definition von Name, Beschreibung, Zutaten, Ergebnis und Vorbereitungszeit
        
    - **IRecipe** als generisches Interface für beliebige Rezeptimplementierungen
        
    - **IngredientEntry** zur serialisierbaren Abbildung einzelner Zutaten mit Item-Referenz und Menge
        
    - **ServiceLocator / PurchaseService** zur Abwicklung von Ressourcen­prüfungen und Inventartransaktionen
        
    - **SyncList** in der CraftingComponent zur Synchronisation bekannter Rezepte über Mirror
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **Serverstart** lädt alle vorhandenen Rezepte und markiert sie als bekannt.
        
    2. **Clientstart** registriert Callback auf die SyncList und baut daraus eine lokale Cache-Liste auf.
        
    3. **CanCraft-Abfrage** auf Client prüft über den PurchaseService, ob genug Ressourcen im Inventar vorhanden sind.
        
    4. **Craft-Aufruf** sendet per Command den Wunsch zum Server, wo Validierung und Ressourcen­abzug sowie Ergebnis­vergabe stattfinden.
        
    5. **OnKnownRecipesChanged-Event** benachrichtigt UI oder andere Systeme über Änderungen der Rezeptliste.
        
- **Architektur- und Designentscheidungen**
    
    - Einsatz von ScriptableObjects für zentrale und editierbare Rezeptdaten
        
    - Verwendung von HashIDs zur einfachen Referenzierung und für Netzwerk­datenstrukturen
        
    - Strikte Trennung von Client- und Server­logik mithilfe von Mirror-Attributen und Commands
        
    - ServiceLocator-Pattern zur Entkopplung der Crafting-Komponente von Inventar- und Ressourcen­logik
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Performance vs. Flexibilität:** Das Wiederaufbauen der gesamten Cache-Liste bei jeder Änderung ist einfach, könnte aber bei vielen Rezepten overhead verursachen.
        
    - **Einfaches Onboarding:** Alle Rezepte sind per Default bekannt, was schnelle Tests erlaubt, aber Freischalt­mechaniken erfordert zusätzliche Logik.
        
    - **Event-basiertes UI-Update:** OnKnownRecipesChanged erlaubt reaktive UI, kann aber zu redundanten Aufrufen führen.
        
    - **Erweiterbarkeit:** Platz für Account- oder Questbasierte Freischaltungen ist eingebaut, aber derzeit nicht implementiert.
        

## Screenshots / Diagramme

- **Sequenzdiagramm Crafting-Workflow:** Client → CanCraft → Cmd_RequestCraft → Server_CanCraft → SERVER_Craft
    
- **Datenflussdiagramm:** SyncList → CachedKnownRecipes → OnKnownRecipesChanged → UI
    
- **UI-Mockup:** Crafting-Fenster mit Filter für bekannte/geblockte Rezepte, Fortschrittsanzeige der Vorbereitung
    

## Tests

1. **Initiale Rezeptsichtbarkeit:** Nach Spielstart sollten alle Rezepte in der UI aufgelistet sein.
    
2. **Ressourcenprüfung:** Bei unzureichenden Materialien muss CanCraft false zurückgeben und Craft-Button deaktivieren.
    
3. **Craft-Durchführung:** Aufruf auf Client löst Command aus, Server entfernt Materialien und fügt Ergebnis ins Inventar ein.
    
4. **Netzwerksynchronisation:** Beim Hinzukommen eines weiteren Clients muss die aktuelle Liste bekannter Rezepte geladen werden.
    
5. **Event-Handling:** Änderungen an SyncList lösen OnKnownRecipesChanged aus und aktualisieren die Darstellung.
    
6. **Zutatenanzeige:** IngredientEntry zeigt korrekte Item-Mengen und Bezeichnung in Logs oder Debug-Ausgaben.
    
7. **Fehlerzustände:** Ungültige oder unbekannte Rezept-Hashes werden serverseitig verworfen und mit Warning-Log gemeldet.
    
8. **Wiederholbarkeit:** Mehrfaches Craften desselben Rezepts funktioniert analog, Mengen werden entsprechend reduziert und erhöht.
    

## Offene Punkte / Probleme

- **Freischaltmechanik fehlt:** Aktuell kennt jeder Spieler alle Rezepte. Implementierung über Quests oder Account-Progression geplant.
    
- **HashID-Kollisionen:** Sicherstellen, dass generierte Hashwerte einzigartig bleiben.
    
- **Optimierung bei vielen Rezepten:** Statt vollständigem Rebuild pro Änderung könnte differenziertes Update sinnvoll sein.
    
- **Vorbereitungszeit:** Aktuell ungenutzt – geplant: visuelles Feedback und Blockierung während Preptime.
    
- **Client-Latenz:** Verzögerungen bei Netzwerkaufrufen könnten Craft-Erlebnis beeinträchtigen; evtl. Prediction/Feedback nötig.