# DILIBAD Lokaler Build – Anleitung (v2.0.17)

Diese Anleitung führt dich Schritt für Schritt durch das Einrichten und Starten des lokal lauffähigen Builds aus dem Release [v-beta-steam-deploy-v2.0.17](https://github.com/DILIBAD/DILIBAD/releases/tag/v-beta-steam-deploy-v2.0.17).

---

## 1. Voraussetzungen

- **Betriebssystem**: Windows 10/11
    
- **.NET Runtime**: .NET 6 Runtime installiert
    
- **Freigegebene Ports** (Firewall):
    
    - Master: TCP 5000
        
    - Room/Session: TCP 5001
        

---

## 2. Release herunterladen und entpacken

1. Lade das ZIP-Archiv des Releases herunter:  
    [https://github.com/DILIBAD/DILIBAD/releases/tag/v-beta-steam-deploy-v2.0.17](https://github.com/DILIBAD/DILIBAD/releases/tag/v-beta-steam-deploy-v2.0.17)
    
2. Entpacke es in zwei Verzeichnisse deiner Wahl, z. B.:
    
    ```
    C:\DILIBAD\Local\
    C:\DILIBAD\Client\
    ```
    

---

## 3. Ordnerstruktur Remote Build

Der remote gehostete Ordner ist analog zum lokalen Aufbau strukturiert, enthält jedoch die Konfiguration für den Remote-Server.

- **Remote-Ordner** (z. B. `Remote/`):
    
    - `MainServer.exe`
        
    - `Session/Session.exe`
        
    - `Client.exe`
        
    - `application.cfg`
        

In der `application.cfg` des Remote-Builds stehen folgende Einstellungen:

```ini
-mstMasterIp=194.62.1.20
-mstMasterPort=5000
-mstStartMaster=True
-mstStartSpawner=True
-mstRoomExe=Remote/Session/Session.exe  ; relativer Pfad hier ausreichend
-mstRoomIp=0.0.0.0                    ; oder spezifische Schnittstelle
-mstRoomDefaultPort=5001
```

Du kannst diese Remote-IP **194.62.1.20** auch im lokalen Build nutzen, um das Spiel gegen den entfernten Server zu testen. Ändere dazu in deiner lokalen `Client/application.cfg` einfach:

```ini
-mstMasterIp=194.62.1.20
-mstMasterPort=5000
```

---

## 4. Main Server konfigurieren

1. Öffne im Ordner `C:\DILIBAD\Local\` die Datei `application.cfg` in einem Texteditor.
    
2. Passe sie wie folgt an:
    
    ```ini
    ; Master-Prozess starten?
    -mstStartMaster=True
    ; IP und Port für den Master (Client verbindet sich hier)
    -mstMasterIp=127.0.0.1
    -mstMasterPort=5000
    
    ; Spawner für Room-Prozesse aktivieren?
    -mstStartSpawner=True
    
    ; Absoluter Pfad zur Session.exe (Room-Prozess)
    -mstRoomExe=C:\DILIBAD\Local\Session\Session.exe
    
    ; IP und Standard-Port für neue Room-Prozesse
    -mstRoomIp=127.0.0.1
    -mstRoomDefaultPort=5001
    ```
    

> **Tipp:** Rechtsklick auf `Session.exe` → „Pfad kopieren“, um Tippfehler zu vermeiden.

---

## 4. Main Server starten

Öffne **PowerShell** oder **Eingabeaufforderung** und führe aus:

```powershell
cd C:\DILIBAD\Local
.\MainServer.exe
```

Du siehst nun die Log-Ausgaben (z. B. „Listening on 127.0.0.1:5000“).

---

## 5. Client konfigurieren

1. Öffne im Ordner `C:\DILIBAD\Client\` die Datei `application.cfg`.
    
2. Ergänze oder passe diese Zeilen an:
    
    ```ini
    -mstMasterIp=127.0.0.1
    -mstMasterPort=5000
    ```
    

Damit liest der Client automatisch die Verbindungsdaten aus der Konfiguration.

---

## 6. Client starten

In einem neuen Terminal:

```powershell
cd C:\DILIBAD\Client
.\Client.exe
```

Der Client verbindet sich nun direkt mit dem lokal laufenden Main Server.

---

## 7. Troubleshooting

- **Keine Verbindung?**
    
    - Prüfe, ob `MainServer.exe` wirklich läuft und in der Konsole „Listening on 127.0.0.1:5000“ anzeigt.
        
    - Erlaube eingehende TCP-Verbindungen auf Port 5000 in deiner Firewall.
        
- **Room/Session-Prozess startet nicht?**
    
    - Überprüfe die `-mstRoomExe`-Einstellung auf korrekten, absoluten Pfad.
        
    - Versuche, die Session manuell zu starten:
        
        ```powershell
        cd C:\DILIBAD\Local\Session
        .\Session.exe --port 5001 --masterIp 127.0.0.1 --masterPort 5000
        ```
        
    - Schau in der Main-Server-Konsole nach Fehlermeldungen.
        
