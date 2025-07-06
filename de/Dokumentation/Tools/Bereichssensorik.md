# Bereichssensorik

# TLDR  
Zentrales Subsystem zur Bereichserfassung in 2D: Ein Overlap-Scanner für flexible Abfragen von Collider-Komponenten und ein Entry/Exit-Tracker zur Überwachung von Objekten beim Betreten bzw. Verlassen von Trigger-Zonen.

## Zweck / Ziel  
Dieses Teilsystem wurde entwickelt, um wiederkehrende Physik-Abfragen und -Events in Unity zentral zu kapseln.  
- **Overlap-Scanner**: Vereinheitlicht Box- und Kreis-Abfragen mit generischem Typ–Safe-Casting und unterschiedlichen Overloads (allokierend, out-basiert, buffer-basiert).  
- **RegionEntryExitTracker**: Verfolgt zuverlässig Ein- und Austritte von beliebigen Component-Typen in Trigger-Bereichen und löst entsprechende Events aus.  
So müssen Projektverantwortliche Anpassungen (z. B. LayerMask, Performanz-Optimierungen) nur an einer Stelle vornehmen.

## Umsetzung  
- **Relevante Kooperationspartner**  
  - UnityEngine.Physics2D & ContactFilter2D für Overlap-Abfragen  
  - Collider2D & TryGetComponent<T> für komponentenbasiertes Filtering  
  - MonoBehaviour & Unity-Trigger-Events (OnTriggerEnter2D / OnTriggerExit2D)  
  - Interfaces IScanArea bzw. IAreaSensor<TDetectable> zur klaren Abstraktion  
- **Ablauf und Interaktionen**  
  1. Aufruf einer Scan-Methode (Box oder Kreis)  
  2. Interne Anwendung von OverlapBox/OverlapCircle mit wahlweiser LayerMask  
  3. Iteration über gefundene Collider, Typprüfung und Rückgabe der ersten bzw. aller Komponenten  
  4. Buffer-Overloads liefern Ergebnisse ohne Heap-Allokationen  
  5. Trigger-basiertes Hinzufügen/Entfernen in der Tracker-Liste und Auslösen von OnEnter/OnExit  
- **Architektur- und Designentscheidungen**  
  - **Generische Overloads**: Maximale Wiederverwendbarkeit für verschiedene Component-Typen  
  - **Allokationsfreie Varianten**: Vermeidung von Garbage Collection bei Performance-kritischen Szenarien  
  - **Out-Parameter** vs. Rückgabe-Array: Flexibilität für unterschiedliche Aufrufer  
  - **Event-basiertes Tracking**: Locker gekoppelte Listener, einfache Integration in Gameplay-Logik  
- **Reflektion & Trade-offs**  
  - Komfort vs. Performanz: Allokierende Methoden sind einfacher, aber teurer  
  - Buffer-Methoden erfordern korrekte Puffergrößen und mehr Setup  
  - Trigger-Tracker verzögert Austrittsmeldungen bei überlappenden Collider-Szenarios

## Screenshots / Diagramme  
- **Sequenzdiagramm**: Overlap-Aufruf → Physics2D → TryGetComponent → Rückgabe  
- **Datenflussdiagramm**: Collider-Puffer → Filter → Ergebnis-Puffer  
- **UI-Vorschlag**: Debug-Gizmos zur Visualisierung der Scan-Volumina im Editor

## Tests  
1. **Basis-Scan**  
   - Szene mit mehreren Test-Objekten in Box/Kreis anlegen  
   - Scan-Methoden (allokierend/out/buffer) aufrufen → korrekte Komponenten finden  
2. **LayerMask-Filter**  
   - Objekte auf unterschiedlichen Layers platzieren  
   - Scan nur für spezifizierte Layer durchführen → nur gewünschte Treffer  
3. **Buffer-Überlauf & -Kapazität**  
   - Puffer bewusst klein wählen und Scan mit mehr Collidern durchführen → keine Exceptions, resultierende Count korrekt  
4. **Event-Tracker**  
   - Test-Objekt mehrfach in Trigger-Zone bewegen  
   - Prüfung: OnEnter/OnExit feuern exakt beim Betreten/Verlassen, Liste spiegelt aktuellen Zustand  
5. **Performanz-Vergleich**  
   - Messung GC-Allokationen aller Overload-Varianten via Profiler

## Offene Punkte / Probleme  
- **3D-Support**: Derzeit nur 2D-Overlap-Abfragen umgesetzt  
- **Dynamische Puffergröße**: Keine automatische Anpassung, manuelle Pufferverwaltung notwendig  
- **Thread-Safety**: Nicht für Multi-Threading geeignet  
- **Mehrfache Events**: Schnelles Hin- und Herbewegen kann zu mehrfachen Einträgen führen, falls Collider nicht eindeutig erkannt werden  
- **Erweiterbarkeit**: Keine zentralisierte Konfiguration für Query-Parameter (z. B. Kontaktfilter-Einstellungen)
