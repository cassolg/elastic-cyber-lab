# Elastic Cyber Lab

This project is an ongoing cybersecurity lab focused on deploying and configuring the Elastic Stack in a self-hosted environment. It simulates real-world threat detection scenarios with secure communication and centralized monitoring.

---

## Project Overview

- Deployed **Elastic Stack** (Elasticsearch, Kibana, Fleet Server) on **Ubuntu 24.04**.
- Manually configured a **TLS-secured Fleet Server** using a custom **Certificate Authority (CA)** and OpenSSL.
- Successfully enrolled an **Elastic Agent on Windows Server 2019**.
- Real-time monitoring via Kibana, enabling endpoint visibility and data ingestion.

---

## Tools & Technologies

| Category         | Tools Used                                     |
|------------------|------------------------------------------------|
| SIEM             | Elastic Stack (Elasticsearch, Kibana, Fleet)   |
| Endpoint Agent   | Elastic Agent on Windows Server 2019           |
| OS/Platform      | Ubuntu 22.04, Windows Server 2019              |
| Security         | TLS, OpenSSL (Manual CA)                       |
| Future Tools     | MITRE ATT&CK, Malware Simulation, Detection Rules |

---

##  Architecture
```bash
[ Kali Linux (Attacker) ]
↓
[ Metasploitable (Vulnerable Machine) ]
↓ (Generates network traffic)
[ Suricata (NIDS) ]
↓ (Generates alerts)
↓
[ Elasticsearch / Kibana (SIEM Stack) ]
```
```bash
[ Windows 10 + Elastic Agent (Sysmon) ]
↓ (Sends logs + receives policy)
[ Fleet Server (via TLS) ]
↓
[ Elasticsearch / Kibana ]
```

<img width="1920" height="1080" alt="Elastic" src="https://github.com/user-attachments/assets/08215972-d0a9-4561-924a-4bb7b40a7417" />

---






*More to come: detection dashboards, simulated attacks, alerts.*
