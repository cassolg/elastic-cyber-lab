# Elastic Cyber Lab

This project is an ongoing cybersecurity lab focused on deploying and configuring the Elastic Stack in a self-hosted environment. It simulates real-world threat detection scenarios with secure communication and centralized monitoring.

---

## Project Overview

- Deployed **Elastic Stack** (Elasticsearch, Kibana, Fleet Server) on **Ubuntu 22.04**.
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

## ⚙️ Architecture
[ Windows Server 2019 ] --> [ Fleet Server (TLS) ] --> [ Elasticsearch / Kibana ]
↑
[ CA + Certs ]

---

## 📸 Screenshots

> ![Elastic Stack Config](images/elk-stack-setup)
> ![Elastic Agent on Windows](images/elastic-agent-installed.png)

*More to come: detection dashboards, simulated attacks, alerts.*
