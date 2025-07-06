### 1. Einleitung

Zu Beginn unseres Unity-Spielprojekts im Sprint 0 haben wir uns in der Gruppe bewusst dafür entschieden, möglichst früh mit Interfaces zu arbeiten. Ziel war es, Abhängigkeiten zwischen noch nicht fertigen oder vollständig implementierten Systemen zu entkoppeln und damit eine parallele Entwicklung zu ermöglichen. Diese Herangehensweise sollte verhindern, dass Aufgaben aufgrund fehlender Komponenten unnötig lange „on hold“ stehen und den Projektfortschritt bremsen.

---

### 2. Vorteile der frühen Interface-Nutzung

1. **Entkopplung von Komponenten**  
    Mit Interfaces konnten wir bereits in frühen Projektphasen Klassen nach außen hin definieren, ohne interne Implementierungen fertigstellen zu müssen. So konnten sich Frontend-Entwickler, Gameplay-Programmierer und Backend-Anbindungen unabhängig voneinander vorarbeiten.
    
2. **Paralleles Arbeiten**  
    Dank klar definierter Schnittstellen war es möglich, einzelne Module wie Inventarsystem, KI-Controller oder Netzwerk-Kommunikation zeitgleich zu entwickeln. Das erhöhte unsere Entwicklungsgeschwindigkeit spürbar.
    
3. **Flexibilität für spätere Änderungen**  
    Theoretisch hätten wir später einzelne Implementierungen austauschen können, ohne alle abhängigen Klassen anzupassen. Das versprach eine erhöhte Modularität und Austauschbarkeit.
    

---

### 3. Herausforderungen und Nachteile

1. **Schwierigkeiten beim Testen**  
    Da konkrete Systemklassen anfangs nicht vorhanden waren, ließen sich einzelne Komponenten nicht direkt testen. Wir mussten auf Helper-Test-Klassen zurückgreifen oder Mock-Objekte schreiben.
    
2. **Verzicht auf Unit Tests**  
    Obwohl Unit Tests die Wartbarkeit und Stabilität unseres Codes langfristig gestärkt hätten, entschieden wir uns gegen ihren Einsatz. Der Grund lag im hohen initialen Aufwand für die Erstellung und Pflege umfangreicher Testklassen. Stattdessen führten wir alle Tests als Teil größerer Integrationstests durch – auf Kosten einer feingranularen Fehlerlokalisierung.
    
3. **Erhöhter Änderungsaufwand**  
    Im weiteren Verlauf stellten wir fest, dass jede Anpassung eines Interfaces oder das Hinzufügen neuer Methoden umfangreiche Refactoring-Arbeiten an allen Implementierungen nach sich zog. Insbesondere bei kleineren, in sich geschlossenen Systemen überstiegen der Aufwand und der Nutzen die Vorteile der Interfaces.
    

---

### 4. Lessons Learned

- **Gezielter Einsatz von Interfaces**  
    In Zukunft würden wir Interfaces primär für die Kommunikation zwischen großen, unabhängigen Systemen einsetzen (z. B. Netzwerk- vs. Gameplay-Modul). Für kleinere, in sich geschlossene Subsysteme empfehlen sich Interfaces nur, wenn eine hohe Komplexität oder potenzielle Mehrfachimplementierungen zu erwarten sind.
    
- **Kriterien und Maßstäbe definieren**  
    Um den Einsatz von Interfaces zu steuern, sollten wir vorab konkrete Kriterien festlegen (z. B. erwartete Anzahl an Implementierungen, Austauschbarkeit in Tests, Schnittstellenkomplexität). Eine abgestufte Entscheidungs­matrix könnte dabei helfen, Aufwand und Nutzen systematisch abzuwägen.
    
- **Balance zwischen Tests und Aufwand**  
    Unit Tests sind ein mächtiges Werkzeug, um Refactoring-Sicherheit und Codequalität zu erhöhen. Auch wenn sie initial Aufwand bedeuten, rechtfertigt dieser Aufwand langfristig Stabilität und Wartbarkeit. Eine minimalistische Test-Strategie (z. B. TDD bei kritischen Komponenten) wäre hier ein guter Mittelweg.
    

---

### 5. Ausblick

Rückblickend hat uns die frühzeitige Nutzung von Interfaces wichtige Erkenntnisse zur Modularität und Parallelisierbarkeit gebracht. Gleichzeitig hat sie gezeigt, dass eine pauschale Verwendung von Interfaces über alle Komponenten hinweg kontraproduktiv sein kann. Für künftige Projekte – insbesondere innerhalb von Unity-Spielprojekten – werden wir durch klar definierte Leitlinien und pragmatischen Interface-Einsatz sowohl die Entwicklungsdynamik als auch die Codequalität optimal ausbalancieren.