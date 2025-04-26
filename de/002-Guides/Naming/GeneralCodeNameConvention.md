---
title: Code Namenkonvention
description: 
published: true
date: 2025-04-23T14:28:51.317Z
tags: 
editor: markdown
dateCreated: 2025-04-23T14:28:51.317Z
---

# C# Naming Conventions for Unity Projects

## 🧠 TL;DR Overview

| Element | Convention | Example |
| --- | --- | --- |
| Classes | PascalCase | `PlayerController` |
| Methods | PascalCase | `MoveForward()` |
| Public Fields | PascalCase | `HealthPoints` |
| Private Fields | _camelCase | `_currentSpeed` |
| Protected Fields | _camelCase | `_isGrounded` |
| Constants | UPPER_CASE_WITH_UNDERSCORES | `MAX_SPEED` |
| Properties | PascalCase | `IsAlive { get; set; }` |
| Events | PascalCase + "On"/"Event" suffix | `OnPlayerDied` |
| Interfaces | I + PascalCase | `IDamageable` |
| Enums | PascalCase | `PlayerState` |
| Enum Members | PascalCase | `Idle`, `Running` |
| Serialized Fields | _camelCase + [SerializeField] | `[SerializeField] private int _health;` |

---

## 🧾 Naming Rules in Detail

### 🧱 Classes

- **Convention:** PascalCase
- **Example:** `PlayerController`, `GameManager`

### ⚙️ Methods

- **Convention:** PascalCase
- **Example:** `MoveForward()`, `Jump()`

### 🌐 Public Fields

- **Convention:** PascalCase
- **Note:** Prefer using properties over public fields
- **Example:** `HealthPoints`, `Score`

### 🔒 Private and Protected Fields

- **Convention:** _camelCase
- **Example:**
  - `private float _currentSpeed;`
  - `protected bool _isGrounded;`

### 💾 Serialized Fields

- **Convention:** _camelCase with `[SerializeField]`
- **Example:**
  
  ```csharp
  [SerializeField] 
  private int _maxHealth;
  ```
  

### 🔁 Constants

- **Convention:** UPPER_CASE_WITH_UNDERSCORES
- **Example:** `MAX_SPEED`, `GRAVITY`

### 🏡 Properties

- **Convention:** PascalCase
- **Example:** `IsJumping { get; private set; }`

### 🔔 Events

- **Convention:** PascalCase with "On" or "Event" suffix
- **Example:** `OnPlayerDied`, `GameOverEvent`

### 🤝 Interfaces

- **Convention:** I + PascalCase
- **Example:** `IDamageable`, `IMovable`

### 🎭 Enums

- **Convention:** PascalCase for type and values
- **Example:**
  
  ```csharp
  public enum PlayerState
  {
      Idle,
      Running,
      Jumping
  }
  ```
  

---

## 🧩 Sample Class with All Conventions

```csharp
using UnityEngine;

public class PlayerController : MonoBehaviour, IDamageable
{
    [SerializeField] 
    private float _moveSpeed = 5f;
    [SerializeField] 
    private int _maxHealth = 100;

    private int _currentHealth;
    private bool _isJumping;

    public int HealthPoints { get; private set; }
    public static readonly float GRAVITY = 9.81f;

    public event System.Action OnPlayerDied;

    private void Start()
    {
        _currentHealth = _maxHealth;
    }

    private void Update()
    {
        Move();
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Jump();
        }
    }

    private void Move()
    {
        // Movement logic
    }

    private void Jump()
    {
        _isJumping = true;
    }

    public void TakeDamage(int amount)
    {
        _currentHealth -= amount;
        if (_currentHealth <= 0)
        {
            OnPlayerDied?.Invoke();
        }
    }
}

public interface IDamageable
{
    void TakeDamage(int amount);
}
```