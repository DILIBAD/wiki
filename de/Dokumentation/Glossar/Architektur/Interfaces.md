
# Interfaces

**Was ist das?**  
Ein Vertrag in C#, der festlegt, welche Methoden und Eigenschaften eine Klasse haben muss, ohne ihre konkrete Umsetzung vorzuschreiben.

- Klassen, die das Interface implementieren, garantieren diese Methoden/Properties.
    
- Ermöglicht Austauschbarkeit und lose Kopplung.
    

**Beispiel in Unity C#:**

```csharp
public interface IDamageable
{
    void TakeDamage(int amount);
}

public class Enemy : MonoBehaviour, IDamageable
{
    public int Health = 50;

    public void TakeDamage(int amount)
    {
        Health -= amount;
        if (Health <= 0) Destroy(gameObject);
    }
}

public class PlayerAttack : MonoBehaviour
{
    void Attack(IDamageable target)
    {
        target.TakeDamage(10);
    }
}
```

**Metapher:**  
Wie eine Steckdose: Egal, ob du einen Haartrockner oder eine Lampe anschließt – solange der Stecker passt (das Interface), funktioniert es.