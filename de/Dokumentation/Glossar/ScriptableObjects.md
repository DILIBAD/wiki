# ScriptableObject

Ein **ScriptableObject** ist in Unity ein spezielles Daten-Container-Format, mit dem man Daten unabhängig von Spielobjekten (GameObjects) speichern und verwalten kann. Man kann sich ein ScriptableObject vorstellen wie eine **digitale Aktenschachtel**, in der man alle wichtigen Einstellungen, Werte und Ressourcen ablegt – losgelöst von einzelnen Figuren oder Szenen.

---

## Warum ScriptableObjects?

- **Zentrale Datenpflege**  
    Anstatt Werte mehrfach in verschiedenen Skripten oder Objekten zu kopieren, legt man sie einmal in einem ScriptableObject ab. Ändert man den Wert, gilt die Änderung überall dort, wo das ScriptableObject genutzt wird.
    
- **Speicherschonend**  
    Da Unity ScriptableObjects als Assets speichert, werden die Daten nicht mehrfach zur Laufzeit geladen, sondern als gemeinsame Instanz verwendet.
    
- **Editor-Integration**  
    Im Unity-Editor erscheinen ScriptableObjects als eigenständige Assets im Projekt-Ordner. Man kann ihre Felder direkt im Inspector bearbeiten, ohne eine Szene laden zu müssen.
    

---

## Aufbau und Verwendung

1. **Erstellung im Code**
    
    ```csharp
    using UnityEngine;
    
    [CreateAssetMenu(menuName = "Spiel/Daten/CharakterStatus")]
    public class CharakterStatus : ScriptableObject
    {
        public string charakterName;
        public int lebenspunkte;
        public float geschwindigkeit;
    }
    ```
    
    Das Attribut `[CreateAssetMenu]` sorgt dafür, dass man im Unity-Editor über **Assets → Create** direkt ein neues `CharakterStatus`-Asset anlegen kann.
    
2. **Befüllen im Editor**  
    Einmal erstellt, klickt man das `CharakterStatus`-Asset an und trägt im Inspector z. B. den Namen „Held“, Lebenspunkte 100 und Geschwindigkeit 5.2 ein.
    
3. **Verwendung im Spiel**
    
    ```csharp
    public class Spieler : MonoBehaviour
    {
        public CharakterStatus status;
    
        void Start()
        {
            Debug.Log($"Name: {status.charakterName}, HP: {status.lebenspunkte}");
        }
    }
    ```
    
    Das Skript `Spieler` referenziert im Inspector das `CharakterStatus`-Asset. So liest es zur Laufzeit die im ScriptableObject definierten Werte aus.
    

---

## Beispiel-Metapher

Stell dir vor, du führst ein **Rezeptbuch** (ScriptableObject).

- Jedes Rezept (Asset) enthält Zutatenliste und Mengenangaben.
    
- Mehrere Köche (Skripte) können dasselbe Rezeptbuch öffnen und daraus kochen.
    
- Änderst du im Rezept das Salz von 1 TL auf 2 TL, merken es sofort alle Köche, ohne dass du jedes einzelne Rezeptblatt manuell bearbeiten musst.
    

---

## Typische Einsatzszenarien

- **Spiel-Settings** (Lautstärke, Grafikqualität)
    
- **Charakter- oder Waffen-Daten** (Schadenswerte, Cooldowns)
    
- **Level-Konfigurationen** (Spawn-Punkte, Zeitlimits)
    
- **Dialog- und Storytexte**
    

---

## Vorteile auf einen Blick

| Vorteil                          | Erklärung                                                                         |
| -------------------------------- | --------------------------------------------------------------------------------- |
| **Wiederverwendbarkeit**         | Einmal definiert, überall nutzbar.                                                |
| **Klarere Projektstruktur**      | Daten sauber von Logik getrennt, Assets übersichtlich im Projekt-Ordner abgelegt. |
| **Einfache Anpassung**           | Werte sind direkt im Editor editierbar, kein erneuter Code-Build nötig.           |
| **Geringerer Speicherverbrauch** | Daten werden nicht dupliziert, sondern als Referenz mehrfach verwendet.           |