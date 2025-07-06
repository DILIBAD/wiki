# EffectComponent

# TLDR

Sorgt für visuelle Rückmeldung im Spiel, indem es Partikel- und Shader-Effekte für Treffer, Buffs und Debuffs netzwerkübergreifend synchronisiert und anzeigt.

## Zweck / Ziel

Die EffectComponent wurde entwickelt, um dem Spieler unmittelbares, visuelles Feedback zu geben, wenn sein Charakter Schaden erleidet oder Statusveränderungen (Buffs/Debuffs) erhält. Sie entkoppelt die reine Gameplay-Logik von der Präsentation und ermöglicht eine zentrale Verwaltung aller visuellen Effekte auf Client- und Serverseite.

## Umsetzung

- **Relevante Kooperationspartner**
    
    - **IDamageableEntity**: Quelle der OnDamageReceived-Events.
        
    - **SyncDictionary<int, double>** (Mirror): Hält aktive Partikeleffekte mit Endzeitpunkt.
        
    - **IVisualEntityEffectTemplate & VisualEntityEffectTemplate.All**: Definiert über Templates, welche Partikel- oder Shader-Effekte abgespielt werden.
        
    - **SpriteRenderer & Material**: Grundlage für Shader-basierte „Flash“- und „Persistent“-Effekte.
        
    - **UniTask** (Cysharp): Asynchrone Warte- und Ablaufsteuerung für Effektdauer.
        
    - **Mirror ClientRpc/Server**: Überträgt Flash- und Persistenteffekte unmittelbar an alle Clients.
        
- **Ablauf und Interaktionen (Unity & Networking)**
    
    1. **Serverseitig**: Beim Start abonniert die Komponente OnDamageReceived.
        
    2. **Effekt auslösen**: SERVER_TriggerParticle fügt im SyncDictionary einen Eintrag mit Template-ID und Endzeit hinzu.
        
    3. **SyncDictionary-Event**: Clients registrieren OnChange und rufen CLIENT_SpawnParticleEffect bzw. CLIENT_RemoveParticleEffect auf.
        
    4. **Flash-/Persistent-Effekte**: Über ClientRpc oder direkt CLIENT_TriggerFlash/CLIENT_ApplyPersistentEffect werden Shader-Animationen gestartet.
        
    5. **Dauerverwaltung**: UniTask-Delays entfernen Einträge nach Ablauf der Laufzeit und setzen Shader-Werte zurück.
        
- **Architektur- oder Designentscheidungen**
    
    - **Template-getriebener Ansatz**: Neue Effekte lassen sich via IVisualEntityEffectTemplate ergänzen, ohne die Komponente selbst anzupassen.
        
    - **SyncDictionary für Partikel**: Garantiert Konsistenz aller aktiven Effekte über Netzwerk und ermöglicht Late-Joins.
        
    - **Shader- vs. Partikeleffekte getrennt**: Flexibilität für kurze „Flash“-Effekte und länger anhaltende Partikel/Shader-Anpassungen.
        
    - **UniTask-Einsatz**: Nicht-blockierende Timings, die unabhängig vom Unity-Update-Loop agieren.
        
- **Reflektion: wichtige Überlegungen und Trade-offs**
    
    - **Netzwerk-Overhead**: SyncDictionary-Einträge verursachen zusätzlichen Datentransfer bei vielen gleichzeitigen Effekten.
        
    - **Performance**: Instanziierung von Partikeln ohne Pooling kann bei häufiger Anwendung Frametimes beeinflussen.
        
    - **Dauer-Drift**: Unterschiedliche Clock-Synchronisierung (NetworkTime vs. lokale Zeit) kann minimale Abweichungen verursachen.
        
    - **Fehlerfall**: Fehlende Templates führen zu Ausnahmen; erweiterte Fallback-Logik könnte stabiler sein.
        


## Tests

1. **Treffer-Feedback**: Serverseitigen Schaden auslösen und prüfen, ob Flash-Shader korrekt sichtbar ist.
    
2. **Partikeleffekt**: Buff- bzw. Debuff-Template triggern, Laufzeit und Position der Partikel validieren.
    
3. **Dauerprüfung**: Effekte mit verschiedenen Laufzeiten abspielen und automatische Entfernung nach Ablauf verifizieren.
    
4. **Mehrfacheffekte**: Überlappende Effekte starten und sicherstellen, dass jeder Partikel-GameObject bleibt, bis sein Timer endet.
    
5. **Late-Join**: Client während eines aktiven Effekts verbinden und kontrollieren, ob der Partikeleffekt nachträglich gespawnt wird.
    
6. **Shader-Reset**: Nach Flash- oder Persistent-Effekt ist der ursprüngliche Shader-Parameter wiederhergestellt.
    
7. **Netzwerkunterbrechung**: Client trennt kurz und reconnectt während eines Effekts; keine Ghost-Effekte oder Duplikate.
    
8. **Fehlerfall**: Template-ID manipulieren und sicherstellen, dass eine aussagekräftige Exception entsteht.
    

## Offene Punkte / Probleme

- **Pooling**: Aktuell keine Wiederverwendung von Partikel-Instanzen; potenzieller GC- und Performance-Nachteil.
    
- **Template-Editor**: Fehlende Unity-Editor-Tools zum visuellen Anlegen und Testen neuer Effect-Templates.
    
- **Drift-Abgleich**: Geringfügige Dauerabweichungen durch Time-Synch-Probleme.
    
- **Fehlerbehandlung**: Robustheit bei fehlenden oder fehlerhaften Templates könnte verbessert werden.
    
- **Icon-Overlay**: Anzeige von Buff/Debuff-Icons im HUD ist nicht Teil dieser Komponente und bleibt extern.