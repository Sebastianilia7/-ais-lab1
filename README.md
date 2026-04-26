# Labb 1

## Steg 4 – Agent
Wazuh-agent installerades på macOS och anslöts till manager.
Agenten registrerades som `sebastian-mac-2` och började skicka loggar.

## Steg 5 – Standardregler
Standardregler i Wazuh observerades via Threat Hunting.
Exempel på alerts:
- SSH login attempts
- Rootcheck (host-based anomaly detection)
- System events (agent start/stop)

## Steg 7 – File Integrity Monitoring (FIM)
FIM (syscheck) aktiverades och konfigurerades för att övervaka `/Users/sebastianilia`.

Efter omstart av agenten genererades säkerhetsevents från agenten, inklusive:
- Host-based anomaly detection (rootcheck)
- System monitoring events

Detta bekräftar att fil- och systemövervakning fungerar och att agenten skickar dessa till Wazuh manager.
