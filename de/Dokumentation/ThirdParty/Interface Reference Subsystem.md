# Interface Reference Subsystem

# TLDR

Ermöglicht das Serialisieren und Validieren von Schnittstellen-Feldern im Unity-Inspektor, inklusive Dropdown-Auswahl von Komponenten und Zuweisungssicherheit.

## Zweck / Ziel

Dieses Teilsystem wurde entwickelt, um die Einschränkung von Unity zu umgehen, dass nur konkrete `Object`-Typen serialisiert werden können. Es löst das Problem, beliebige `interface`-Referenzen in Script-Feldern darzustellen, zu speichern und im Editor zu validieren, ohne dass dafür eigene Wrapper-Objekte oder umfangreiche Boilerplate nötig sind.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **InterfaceReference<TInterface, TObject> & InterfaceReference**: Generischer Container, der ein `UnityEngine.Object` intern hält und bei Zugriff auf das Interface umwandelt.
        
    - **RequireInterfaceAttribute**: Attribut zum Markieren von Feldern, die zwingend ein Objekt mit einer bestimmten Interface-Implementierung benötigen.
        
    - **InterfaceReferenceDrawer & RequireInterfaceDrawer**: Custom Property Drawers, die im Inspektor die richtige UI für Interface-Zuweisungen und -Validierungen bereitstellen.
        
    - **InterfaceReferenceUtil**: Hilfsklasse für das Zeichnen eines kleinen Labels neben dem Feld, das das Interface-Symbol anzeigt oder bei Hover den Interface-Namen ausgibt.
        
- **Ablauf und Interaktionen**
    
    1. Beim Anzeigen des Inspektors erkennt der Drawer über Reflection den generischen Typ und das geforderte Interface bzw. Objekt-Typ.
        
    2. Wird ein `GameObject` zugewiesen, listet der Drawer alle Komponenten auf, die das Interface implementieren, und bietet sie in einem Popup an.
        
    3. Bei direkter Zuweisung eines Komponenten- oder ScriptableObject-Objekts prüft der Drawer, ob dessen Typ das Interface erfüllt; andernfalls wird die Zuweisung abgelehnt und eine Warnung geloggt.
        
    4. Das `underlyingValue`-Feld speichert intern die tatsächliche Unity-Referenz. Die `Value`-Property castet beim Get/Set zwischen Interface und Objekt.
        
- **Architektur- oder Designentscheidungen**
    
    - **Generische Typen**: Trennung von Interface- und Objekt-Typen ermöglicht breite Wiederverwendbarkeit.
        
    - **Editor-Only-Code**: Alle Drawers und Util-Klassen sind auf `#if UNITY_EDITOR` beschränkt, um keinen Einfluss auf Builds zu nehmen.
        
    - **Kein Code-Referenzieren**: Fokus auf Reflection und SerializedProperty, um keine direkten Abhängigkeiten zu den konkreten Skripten im Projekt zu haben.
        
    - **Usability**: Dropdown für GameObjects mit mehreren passenden Komponenten, klares Feedback bei ungültigen Zuweisungen.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - Reflection wirkt sich minimal auf die Editor-Performance aus, ist aber unkritisch für Release-Builds.
        
    - Bindung an `UnityEngine.Object` schließt reine POCO-Interfaces aus, unterstützt jedoch alle Standard-Unity-Typen.
        
    - Kein direkter Support für Collections mit `InterfaceReference<>`, außer in der Require-Drawer-Implementierung von Arrays und `List<>`.
        

## Screenshots / Diagramme

- **Inspektor-Darstellung**: Feld mit Label `(IMyInterface)` und Popup-Auswahl bei GameObject-Zuweisung.
    
- **Ablaufdiagramm**:
    
    1. `OnGUI` → 2. Typ-Ermittlung via Reflection → 3. Prüfung/Popup-Füllung → 4. Zuweisung oder Warnung → 5. `InterfaceReferenceUtil`-Label
        
- **Datenfluss**:
    
    - Feldwert (`SerializedProperty`) ↔ `underlyingValue` ↔ `Value`-Property ↔ User-Skript
        

## Tests

1. **Zuweisung gültiger Komponente**
    
    - Feld auf Interface-Typ `IMyInterface` setzen.
        
    - GameObject mit `MyComponent : MonoBehaviour, IMyInterface` zuweisen → Komponente wird automatisch ausgewählt.
        
2. **Zuweisung ungültiger Komponente**
    
    - GameObject ohne passende Komponente zuweisen → Feld wird geleert, Warnung im Console-Log.
        
3. **Direkte Objekt-Zuweisung**
    
    - Monobehaviour- oder ScriptableObject-Asset, das das Interface implementiert, direkt ins Feld ziehen → Zuweisung erfolgt.
        
    - Andernfalls Warnung und Löschung der Zuweisung.
        
4. **Array-/Listen-Unterstützung**
    
    - `RequireInterfaceAttribute` auf `IMyInterface[]` und `List<IMyInterface>` anwenden.
        
    - Array-Größe verändern, Elemente einzeln validieren.
        
5. **Hover-Feedback**
    
    - Mouseover über das Feld zeigt `(IMyInterface)`-Label bzw. `*` bei gültiger Zuweisung.
        

## Offene Punkte / Probleme

- **Nur im Editor**: Keine Laufzeit-Validierung oder Serialisierung außerhalb des Inspektors.
    
- **Performance**: Reflection könnte bei zahlreichen Feldern spürbar werden.
    
- **Eingeschränkte Collection-Unterstützung**: `InterfaceReference<>` selbst unterstützt keine Collections; nur Require-Drawer behandelt Arrays/Listen.
    
- **Interface-Implementierungen in Prefabs**: Änderungen an Prefabs müssen neu validiert werden, wenn sich die Komponentenzusammensetzung ändert.
    

## Quellen

- Video: _Serialize Interfaces in Unity_ von git-amend – [https://www.youtube.com/watch?v=xcGPr04Mgm4](https://www.youtube.com/watch?v=xcGPr04Mgm4)
    
- GitHub Repository: _Unity-Serialized-Interface_ – [https://github.com/adammyhre/Unity-Serialized-Interface](https://github.com/adammyhre/Unity-Serialized-Interface)