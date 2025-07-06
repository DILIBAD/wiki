# NPC State Machine

Jeder NPC im Spiel wird durch eine State Machine gesteuert, die sieben verschiedene Zustände (States) umfasst:

- **Idle State:**
    
    Die NPCs haben kein aktuelles Ziel, bewegen sich nicht und befinden sich in einem Ruhezustand. Während dieses States steigt ein "Langeweile"-Wert, der nach Erreichen eines bestimmten Schwellenwerts den Wechsel in den Wandering State auslöst.
    
- **Wandering State:**
    
    NPCs bewegen sich zu einem neuen Zielpunkt, um ihre Umgebung zu erkunden und lebendiger zu wirken. Dieser State legt lediglich das Bewegungsziel fest, das eigentliche Bewegen wird im Movement State ausgeführt.
    
- **Movement State:**
    
    Verantwortlich für die generelle Fortbewegung der NPCs zum festgelegten Zielpunkt.
    
- **PickSkill State:**
    
    Die NPCs wählen in regelmäßigen Abständen eine Fähigkeit aus und bereiten diese vor.
    
- **Attack State:**
    
    In diesem State führen die NPCs die ausgewählten Fähigkeiten aus und bewegen sich dabei aktiv auf Spieler zu.
    
- **Enrage State (boss-spezifisch):**
    
    Dieser State stellt einen Damage-Check dar. Nach Entdeckung des ersten Spielers startet ein Timer. Läuft dieser ab, ohne dass der Boss besiegt wurde, wird ein mächtiger Angriff ausgelöst, der die gesamte Arena betrifft. Die Spieler haben eine letzte Chance, während der Ladephase des Angriffs, den Boss mit maximalem Schaden zu besiegen. Schafft der Boss es, den Angriff durchzuführen, wird die gesamte Spielergruppe besiegt. Dieser Mechanismus sorgt für erhöhte Spannung und fordert gutes Teamplay.
    
- **Invulnerability (Invuln) State (boss-spezifisch):**
    
    In bestimmten Lebensabschnitten wird der Boss unbesiegbar und führt eine Weltaktion durch — eine Mechanik, die von den Spielern Ausweichmanöver erfordert. Nach Abschluss dieser Phase ist der Boss wieder verwundbar.
    

---

