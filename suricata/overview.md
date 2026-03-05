#  Suricata Network Intrusion Detection System (IDS)

##  Overview

As part of the Elastic Cyber Lab, Suricata was deployed to provide **network-level visibility** and serve as a packet inspection sensor for the SOC environment.

Suricata operates as a **Network Intrusion Detection System (NIDS)** and monitors traffic between simulated attacker and target systems within an isolated lab network.

This component enables:

* Deep Packet Inspection (DPI)
* Protocol analysis (HTTP, DNS, TLS, SSH, etc.)
* Signature-based intrusion detection
* Structured security event logging
* Integration with SIEM for centralized analysis

---

##  Purpose in the Lab

While Elasticsearch and Kibana provide analysis and visualization, Suricata acts as the **network sensor** responsible for detecting suspicious traffic patterns.

It bridges the gap between:

Network Activity → Security Events → SIEM Analysis

This allows the lab to simulate real-world SOC architecture where IDS sensors feed centralized monitoring platforms.

---

##  Deployment Architecture

```text
Attacker (Kali Linux)
        │
        ▼
Target (Metasploitable)
        ▲
        │
Suricata IDS (Packet Inspection)
        │
        ▼
Filebeat → Elasticsearch → Kibana
```

---

##  Key Capabilities Enabled

###  Network Traffic Monitoring

Suricata passively inspects network packets using promiscuous-mode capture.

###  Protocol Awareness

Traffic is decoded and categorized by application-layer protocol.

###  Structured Event Logging

Security events are exported in JSON format for SIEM ingestion.

###  Detection Engine

Signature-based rules identify suspicious patterns such as scanning, exploitation attempts, and anomalous behavior.

---

##  Integration with Elastic Stack

Suricata logs are forwarded to Elasticsearch via Filebeat, allowing:

* Centralized event storage
* Threat hunting
* Correlation with other logs
* Dashboard visualization
* Case creation for investigations

This enables a realistic SOC workflow from detection to investigation.

---

##  Validation

Suricata successfully:

* Monitored live traffic within the attack network
* Generated protocol logs and alerts
* Exported structured JSON logs
* Delivered events to the Elastic Stack

This confirms full operational status of the network monitoring sensor.

---

