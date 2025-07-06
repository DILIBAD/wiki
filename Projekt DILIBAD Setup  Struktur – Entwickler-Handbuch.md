## Projekt DILIBAD: Setup & Struktur – Entwickler-Handbuch

### Inhaltsverzeichnis

1. Voraussetzungen
    
2. Unity-Version einrichten
    
3. Repository klonen & öffnen
    
4. Projektstruktur im Überblick
    
    - 4.1 Wichtige Ordner
        
    - 4.2 _Project-Verzeichnis im Detail
        
5. Entwicklungs-Workflows
    
6. Commit-Best Practices
    

---

## 1. Voraussetzungen

- **Unity Hub**: Verwaltung verschiedener Unity-Versionen.
    
- **Git & GitHub Desktop**: Version Control, empfohlen für Nutzerfreundlichkeit und Umgang mit Unity-Meta-/.asset-Dateien.
    

## 2. Unity-Version einrichten

1. Unity Hub → **Installs** → **Add**.
    
2. Version **6000.0.46f1** auswählen oder aus dem Unity-Archiv herunterladen.
    
3. Installation bestätigen und im Hub verfügbar machen.
    

## 3. Repository klonen & öffnen

1. GitHub Desktop → **File → Clone repository** → Repo-URL eingeben.
    
2. Nach dem Klonen über **Repository → Open in Unity Hub** öffnen oder im Hub **Projects → Add** → Root-Ordner wählen.
    
3. **Root-Ordner** des Repos öffnen, damit Unity Assets, ProjectSettings und Packages findet.
    

> **Hinweis:** Erster Start kann bis zu einer Stunde dauern (Asset-Import, Shader-/Skript-Compile).

## 4. Projektstruktur im Überblick

### 4.1 Wichtige Ordner

```
DILIBAD/                   ← Root des Git-Repos
│
├─ Assets/                 ← Unity-Assets
│   ├─ _Project/           ← Eigene Scripts, Prefabs & Ressourcen
│   ├─ Scenes/             ← Haupt- und Dev-Personal-Szenen
│   ├─ Resources/          ← Laufzeit-dynamisch ladbare Assets
│   └─ ThirdParty/         ← Externe Plugins & Bibliotheken
│
├─ ProjectSettings/        ← Unity-Projektkonfiguration
├─ Packages/               ← Unity-Package-Manager-Abhängigkeiten
└─ README.md, .gitignore
```

### 4.2 _Project-Verzeichnis im Detail

```
_Project/                 ← Eigene Module & Inhalte
├─ Scripts/              ← C#-Skripte in Modulen mit .asmdef
│   ├─ ModuleA/          ← Assembly Definition für schnellen Compile
│   └─ ModuleB/
│
├─ Prefabs/              ← GameObject-Vorlagen
│   ├─ Entities/         ← Charaktere, NPCs
│   └─ UI/               ← Benutzeroberfläche
│
├─ Animations/           ← Animation-Clips & Controller
├─ Audio/                ← Musik, Effekte, UI-Sounds
├─ Material/             ← Shader & Materialien
└─ Other Resources       ← NavMeshes, Texturen, etc.
```

## 5. Entwicklungs-Workflows

### 5.1 Skript-Komponenten & Assembly Definitions

- **Modulare Kompilierung**: .asmdef-Dateien reduzieren Recompile-Zeit.
    
- **Hot Reload**: Code-Änderungen direkt patchen; komplette Recompile via `Strg+R` oder **Window → HotReload → Recompile**.
    

### 5.2 Szenen & Merge-Konflikte

- Dev-Personal-Space in **Assets/_Project/Scenes/Dev/Personal Space/**:  
    Jeder Entwickler legt eigene Szenen an, um Konflikte in der Hauptszene zu vermeiden.
    

### 5.3 Ressourcen laden

- **Resources/**-Ordner für `Resources.Load<T>("Pfad/Name")`.
    

### 5.4 Drittanbieter-Pakete

- **ThirdParty/** trennt externe Assets klar vom eigenen Code.
    

## 6. Commit-Best Practices

1. Vor jedem Commit in Unity **File → Save Project** drücken.
    
2. GitHub Desktop: Alle Änderungen inkl. `.meta` und `.asset` tracken.
    
3. Prägnante Commit-Messages (z. B. „Add PlayerMovement Module“).
    
4. Keine temporären Builds ins Repo legen.
