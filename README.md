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

## Steg 6 – Egna detektionsregler

Eftersom jag använder kursens gemensamma Wazuh-manager har jag inte adminåtkomst till `local_rules.xml` på managern. Därför dokumenteras egna detektionsregler som egna detektionskriterier baserade på loggar och FIM-events från min agent.

| Regel | Trigger | Allvarlighet | Beskrivning |
|---|---|---|---|
| Custom Rule 1 | `sudo`-aktivitet från macOS-loggar | Medium | Upptäcker administrativ aktivitet på endpointen. |
| Custom Rule 2 | Ändring i `/Users/sebastianilia` via FIM/syscheck | High | Upptäcker ändringar i övervakad användarkatalog. |
| Custom Rule 3 | Ovanliga host/rootcheck-events | Medium | Upptäcker host-baserade avvikelser och säkerhetskontroller. |

## Steg 7 – File Integrity Monitoring (FIM)
FIM (syscheck) aktiverades och konfigurerades för att övervaka `/Users/sebastianilia`.

Efter omstart av agenten genererades säkerhetsevents från agenten, inklusive:
- Host-based anomaly detection (rootcheck)
- System monitoring events

Detta bekräftar att fil- och systemövervakning fungerar och att agenten skickar dessa till Wazuh manager.

## Steg 8 – Python anomalidetektering

Ett enkelt Python-skript skapades för att analysera säkerhetsloggar.

Loggar med nivå ≥ 5 klassificeras som avvikande (anomaly), medan lägre nivåer anses vara normal aktivitet.

Exempel på resultat:
- sudo command → ANOMALY
- file change → ANOMALY
- normal activity → NORMAL

## Reflektion

Under labben lärde jag mig hur man:
- installerar och konfigurerar en Wazuh-agent
- analyserar säkerhetsloggar i ett SIEM-system
- använder FIM för att upptäcka filändringar
- bygger enkel anomalidetektering med Python

Utmaningar:
- Agent-anslutning och autentisering
- XML-konfiguration
- Felsökning av FIM och loggflöde
