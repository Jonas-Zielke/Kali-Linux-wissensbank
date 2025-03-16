# Windows Privilege Escalation Techniken

### 1. Einleitung

Privilege Escalation bezeichnet den Vorgang, mit dem ein Angreifer aus einer zunächst eingeschränkten Benutzerumgebung (z. B. einer kompromittierten Benutzer-Shell) höhere Rechte, etwa SYSTEM- oder Administratorrechte, erlangt. Bei Windows gibt es verschiedene Techniken, die Schwachstellen im System oder Fehlkonfigurationen ausnutzen.

***

### 2. Vorbereitungen und Informationsbeschaffung

Bevor du konkrete Techniken anwendest, solltest du möglichst viele Informationen über das Zielsystem sammeln:

* **Systeminformationen:**\
  Führe in der aktuellen Shell Befehle wie `systeminfo`, `whoami /priv` oder `net user` aus, um einen Überblick über das System, installierte Updates und aktuelle Berechtigungen zu erhalten.
*   **Tools zur Erkennung von Schwachstellen:**\
    Nutze Tools wie **winPEAS**, **PowerUp**, oder **Sherlock**, die automatisiert nach möglichen Schwachstellen und Fehlkonfigurationen suchen.\
    Beispiel:

    ```powershell
    .\winPEASany.exe
    ```

***

### 3. Häufige Privilege Escalation Techniken

#### 3.1 Unquoted Service Paths

**Beschreibung:**\
Viele Windows-Dienste werden mit unvollständig umschlossenen Pfadangaben (ohne Anführungszeichen) konfiguriert. Falls ein Dienstpfad Leerzeichen enthält, interpretiert das System den Pfad falsch, was Angreifern erlaubt, eine eigene ausführbare Datei in einem „gefährdeten“ Verzeichnis zu platzieren, die dann unter SYSTEM-Rechten ausgeführt wird.

**Vorgehensweise:**

*   **Identifikation:**\
    Überprüfe Dienstpfade mithilfe von Tools wie `wmic`:

    ```cmd
    wmic service get name,displayname,pathname,startmode | findstr /i "Auto"
    ```
* **Ausnutzung:**\
  Wenn ein unquoted Pfad gefunden wird, kann ein Angreifer eine präparierte ausführbare Datei an der entsprechenden Stelle ablegen. Dies erfordert oft Schreibzugriff auf das betroffene Verzeichnis.

***

#### 3.2 Schwache Dateisystemberechtigungen

**Beschreibung:**\
Fehlkonfigurierte Dateiberechtigungen (ACLs) können es Angreifern ermöglichen, kritische Dateien oder Dienste zu manipulieren oder zu ersetzen, die unter erhöhten Rechten laufen.

**Vorgehensweise:**

*   **Überprüfung:**\
    Tools wie **AccessChk** von Sysinternals helfen, schwache Berechtigungen zu identifizieren:

    ```cmd
    accesschk.exe -uws "C:\Program Files\SomeService\service.exe"
    ```
* **Ausnutzung:**\
  Falls du Schreibzugriff auf die Datei oder das Verzeichnis erhältst, kannst du eine modifizierte Version (z. B. mit eingebettetem Payload) platzieren.

***

#### 3.3 DLL-Hijacking

**Beschreibung:**\
Viele Anwendungen laden DLLs (Dynamic Link Libraries) zur Laufzeit. Wird eine DLL in einem unwahrscheinlichen Verzeichnis gesucht, kann ein Angreifer eine manipulierte DLL erstellen, die dann statt der legitimen Version geladen wird.

**Vorgehensweise:**

* **Identifikation:**\
  Analysiere, welche DLLs von Anwendungen ohne absoluten Pfad geladen werden. Tools wie **Process Monitor** (Procmon) können hier helfen.
* **Ausnutzung:**\
  Erstelle eine manipulierte DLL mit demselben Namen und platziere sie in einem Verzeichnis, aus dem die Anwendung sie lädt.

***

#### 3.4 Dienst-Fehlkonfigurationen und -Manipulation

**Beschreibung:**\
Einige Windows-Dienste laufen unter SYSTEM-Rechten, erlauben aber unzureichende Schreibrechte an ihren Konfigurationsdateien oder ausführbaren Dateien.

**Vorgehensweise:**

* **Identifikation:**\
  Suche mit Tools wie **PowerUp** nach Diensten, die manipuliert werden können.
* **Ausnutzung:**\
  Ersetze die ausführbare Datei eines Dienstes oder injiziere eigenen Code, der bei einem Neustart des Dienstes unter SYSTEM-Rechten ausgeführt wird.

***

#### 3.5 Token-Impersonation und Handle Duplication

**Beschreibung:**\
Token-Impersonation nutzt bestehende Sicherheits-Token eines privilegierten Prozesses, um dessen Rechte zu übernehmen. Tools wie **Incognito** (Teil des Metasploit Frameworks) können dir helfen, privilegierte Token zu extrahieren und zu duplizieren.

**Vorgehensweise:**

* **Überprüfung:**\
  Mit dem Befehl `whoami /priv` kannst du sehen, welche Privilegien bereits aktiviert sind.
*   **Ausnutzung:**\
    Nutze Tools oder Skripte, um privilegierte Token zu duplizieren und zu übernehmen. Beispiel (über Metasploit):

    ```ruby
    meterpreter > list_tokens -u
    meterpreter > impersonate_token <TOKEN_ID>
    ```

***

#### 3.6 UAC-Bypass (User Account Control)

**Beschreibung:**\
UAC-Bypass-Techniken zielen darauf ab, die Benutzerkontensteuerung von Windows zu umgehen, um erhöhte Rechte zu erlangen. Einige Techniken nutzen legitime Windows-Binaries aus, die bei bestimmten Konfigurationen fehlerhaft arbeiten.

**Vorgehensweise:**

*   **Beispiel – Fodhelper UAC Bypass:**\
    Viele UAC-Bypass-Methoden beinhalten das Ausnutzen von Registry-Einträgen in Verbindung mit Fodhelper.exe:

    1.  Setze in der Registry einen schädlichen Befehl:

        ```cmd
        reg add HKCU\Software\Classes\ms-settings\shell\open\command /d "cmd.exe /c <dein_befehl>" /f
        ```
    2.  Starte Fodhelper.exe:

        ```cmd
        fodhelper.exe
        ```

    Dabei wird der Befehl mit erhöhten Rechten ausgeführt.\
    &#xNAN;_&#x48;inweis:_ Diese Methode kann von aktuellen Windows-Versionen und Sicherheitspatches bereits geschlossen sein.

***

#### 3.7 Nutzung von Exploits für Kernel-Schwachstellen

**Beschreibung:**\
Kernel-Exploits zielen auf Schwachstellen im Windows-Kernel ab, um SYSTEM-Rechte zu erlangen. Diese Exploits sind oft auf bestimmte Windows-Versionen beschränkt und sollten nur in Testumgebungen verwendet werden.

**Vorgehensweise:**

*   **Beispiel:**\
    Nutze Tools wie **Windows Exploit Suggester** zur Identifikation von möglichen Kernel-Schwachstellen:

    ```cmd
    python windows_exploit_suggester.py --update
    python windows_exploit_suggester.py --database <path_to_db> --systeminfo systeminfo.txt
    ```
* **Ausnutzung:**\
  Sobald eine passende Schwachstelle identifiziert wurde, kann ein entsprechender Exploit (häufig als PoC veröffentlicht) versucht werden. Beachte, dass der Einsatz solcher Exploits in der Regel riskant und schwerwiegender als andere Methoden ist.

***

### 4. Wichtige Tools zur Privilege Escalation

* **PowerUp:**\
  Ein PowerShell-Skript, das häufige Konfigurationsfehler und Schwachstellen im System aufzeigt.
* **winPEAS:**\
  Ein portables Tool, das zahlreiche Windows-Privilegien und Schwachstellen überprüft.
* **Sherlock:**\
  Ein weiteres Skript zur automatisierten Suche nach Privilege Escalation Möglichkeiten auf Windows-Systemen.
* **AccessChk:**\
  Ein Tool von Sysinternals, um Dateisystem- und Dienstberechtigungen zu prüfen.
* **Incognito (Metasploit):**\
  Ermöglicht die Anzeige und Übernahme von Sicherheits-Token innerhalb einer Meterpreter-Session.

***

### 5. Praktisches Beispiel: Unquoted Service Path ausnutzen

1.  **Dienstpfade prüfen:**

    ```cmd
    wmic service get name,displayname,pathname,startmode | findstr /i "Auto"
    ```
2.  **Unquoted Pfad identifizieren:**\
    Angenommen, ein Dienstpfad lautet:

    ```
    C:\Program Files\Vulnerable Service\service.exe
    ```

    Ohne Anführungszeichen kann Windows den Pfad als mehrere Einträge interpretieren, wenn z. B. „C:\Program“ ein existierendes Verzeichnis ist.
3. **Manipulation:**\
   Erhalte Schreibrechte im Verzeichnis `C:\Program Files\Vulnerable Service\` und platziere eine manipulierte `service.exe` (z. B. mit einem Payload), die bei einem Dienstneustart ausgeführt wird.
