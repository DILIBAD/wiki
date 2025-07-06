# Purchase Service


# TLDR  
Zentrale Prüf- und Ausführungslogik für Käufe unter Nutzung von lokalem und globalem Inventar.

## Zweck / Ziel  
Der `PurchaseService` wurde geschaffen, um Kaufentscheidungen und -vorgänge konsistent zu implementieren. Er beantwortet, ob ein Spieler die Kosten decken kann, und führt daraufhin die Ressourcenkorrektur im lokalen und (falls zugelassen) im globalen Inventar durch.

## Umsetzung  
- **Relevante Kooperationspartner**  
  - `IItemContainer` (lokales Inventar des Spielers)  
  - `ServiceLocator.GlobalInventory` (globaler Ressourcenpool)  
  - Kosten-Definitionen via `IItemCostTemplate`  
  - Events für Entnahmevisualisierung (`OnInventoryRemovedItems*`)  

- **Ablauf und Interaktionen**  
  1. **Prüfung (CanFulfill/CanPurchase)**  
     - Iterative Validierung aller Kostenpositionen (lokal → global).  
     - Rückgabe eines Bool für die Suffizienz der Ressourcen.  
  2. **Ausführung (Purchase)**  
     - Entfernen von Ressourcen in zwei Schritten: zuerst lokal, dann global (Rest).  
     - Auf Client-Seite werden Entnahme-Events mit Transform- oder Position-Angabe ausgelöst.  
     - Auf Server-Seite direkte Entfernung über `SERVER_RemoveById` bzw. global über ServiceLocator.  

- **Architektur- oder Designentscheidungen**  
  - **Getrennte Client-/Server-Methoden**: Vermeidung von Netzwerklatenz bei Validierung vs. Autorisierung.  
  - **Event-Mechanismus**: Übergabe von Transform/Position für flexible Darstellung von Items beim Droppen.  
  - **Separation of Concerns**: Logik für Prüfungen und Ausführung nicht im Inventar-Container, sondern eigenständig im Service.

- **Reflektion: wichtige Überlegungen und Trade-offs**  
  - **Vorteil**: Einheitliche, wiederverwendbare Kauflogik, keine redundanten Abfragen in Subsystemen.  
  - **Nachteil**: Duplizierte Methoden für Client und Server erhöhen Pflegeaufwand.  
  - **Risiko**: Fehlende Transaktionssicherheit bei Teilausführungen, keine Rollback-Mechanismen bei Teilfehlern.


## Tests  
1. **Validierungsprüfung**  
   - Spieler hat genau Kostenmenge lokal → `CanFulfill` true.  
   - Fehlende lokale + globale Ressourcen → `CanFulfill` false.  
2. **Kombinierte Entnahme**  
   - Teil lokal, Teil global → lokale Entnahme, danach globale, Events korrekt ausgelöst.  
3. **Event-Empfang**  
   - UI/Spawners abonnieren `OnInventoryRemovedItemsOnTransform` bzw. `On…OnPosition` → Animation/Drop erscheint an richtiger Stelle.  
4. **Server-Ausführung**  
   - Autorisierte Server-Methode entfernt Ressourcen, kein Client-Event notwendig.  
5. **Fehlerfälle**  
   - Aufruf von Purchase ohne vorherige Prüfung → negative Bestände vermeiden, Service verhält sich robust.

## Offene Punkte / Probleme  
- **Atomare Operationen**: Keine eingebaute Transaktion, Teilentnahmen könnten unvollständig sein.  
- **Fehler-Feedback**: Keine differenzierte Fehlermeldung bei fehlgeschlagenen Käufen.  
- **Code-Duplication**: Methoden für CLIENT/SERVER weitgehend identisch – Refactoring-Potenzial.  
- **Concurrency**: Mehrere parallele Kaufversuche gleichzeitig könnten Inkonsistenzen erzeugen.  
