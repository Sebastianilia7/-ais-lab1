# Labb 1

## Steg 4 – Agent
Wazuh-agent installerades på macOS och anslöts till manager.  
Agenten registrerades som `sebastian-mac-2` och började skicka loggar.

---

## Steg 5 – Standardregler
Standardregler i Wazuh observerades via Threat Hunting.  
Exempel på alerts:
- SSH login attempts  
- Rootcheck (host-based anomaly detection)  
- System events (agent start/stop)  

---

## Steg 6 – Egna detektionsregler

Eftersom jag använder kursens gemensamma Wazuh-manager har jag inte adminåtkomst till `local_rules.xml`.  
Därför implementerades egna detektionsregler genom Python-baserad logganalys.

Följande regler implementerades:

### Regel 1 – Suspicious sudo activity
- Trigger: loggar som innehåller "sudo"  
- Åtgärd: klassificeras som ANOMALY  

### Regel 2 – File change detection
- Trigger: filändringar i `/Users/sebastianilia`  
- Åtgärd: klassificeras som ANOMALY  

### Regel 3 – Low severity filtering
- Trigger: events med nivå < 5  
- Åtgärd: klassificeras som NORMAL  

Dessa regler implementerades i Python-skriptet och testades med simulerad loggdata.

---

## Steg 7 – File Integrity Monitoring (FIM)
FIM (syscheck) aktiverades och konfigurerades för att övervaka `/Users/sebastianilia`.

Efter omstart av agenten genererades säkerhetsevents från agenten, inklusive:
- Host-based anomaly detection (rootcheck)  
- System monitoring events  

Detta bekräftar att fil- och systemövervakning fungerar och att agenten skickar dessa till Wazuh manager.

---

## Steg 8 – Python anomalidetektering

Ett enkelt Python-skript skapades för att analysera säkerhetsloggar.

Loggar med nivå ≥ 5 klassificeras som avvikande (anomaly), medan lägre nivåer anses vara normal aktivitet.

Exempel på resultat:
- sudo command → ANOMALY  
- file change → ANOMALY  
- normal activity → NORMAL  

### Resultat från skriptet
<img width="573" height="87" alt="Skärmavbild 2026-04-26 kl  16 33 18" src="https://github.com/user-attachments/assets/97267fe0-e45a-4948-a328-fcd22ae49e3a" />

---


## Dashboard

Wazuh Dashboard användes för att visualisera säkerhetshändelser från agenten.

I Threat Hunting kunde jag se:
- Events från agenten `sebastian-mac-2`  
- Rootcheck och system events  
- Loggdata som analyserats av Wazuh  

En skärmdump bifogas som visar säkerhetshändelser i realtid.

<img width="1461" height="759" alt="Skärmavbild 2026-04-26 kl  16 26 04" src="https://github.com/user-attachments/assets/51987c34-be87-4dec-a79d-b343d2002459" />

---

## Automatiserad incidentrespons

Wazuh har stöd för automatiserad incidentrespons genom "active response".

I denna labb simulerades incidentrespons genom Python-skriptet, där:
- loggar med hög nivå (≥5) klassificeras som ANOMALY  
- detta representerar ett automatiserat beslutssystem som kan trigga åtgärder  

Exempel på verkliga åtgärder:
- blockera IP-adresser  
- stoppa processer  

---

## Arkitektur

Systemet består av följande komponenter:

- Wazuh Agent (macOS)  
  Samlar loggar och skickar till manager  

- Wazuh Manager  
  Analyserar loggar och applicerar regler  

- Wazuh Indexer  
  Lagrar loggdata  

- Wazuh Dashboard  
  Visualiserar säkerhetshändelser  

### Dataflöde

Agent (macOS)
↓
Wazuh Manager
↓
Wazuh Indexer
↓
Wazuh Dashboard

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
