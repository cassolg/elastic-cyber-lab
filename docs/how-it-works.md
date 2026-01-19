#  How It Works

This lab simulates a real-world detection and monitoring environment using the Elastic Stack, Suricata NIDS, Elastic Agent, and attacker/target machines.

---

##  Components and Roles

###  Elastic Stack (Ubuntu Host)
- **Elasticsearch** – stores all logs and indexed data.
- **Kibana** – visualizes logs, dashboards, and detections.
- **Logstash** – parses and enriches incoming logs.
- **Filebeat** – forwards logs from Suricata to Logstash.

###  Windows 10 Endpoint
- Runs **Elastic Agent**
- Collects:
  - Windows Event Logs
  - Sysmon (if configured)
  - Security telemetry
- Sends logs directly to the Elastic Stack.

###  Suricata NIDS
- Monitors network traffic between machines.
- Detects suspicious activity (e.g., port scans, exploits).
- Outputs alerts to log files (e.g., `eve.json`)
- **Filebeat** ships Suricata logs to Logstash/Elasticsearch.

###  Kali Linux Attacker
- Used to simulate real-world attacks like:
  - Reconnaissance
  - Exploitation
  - Credential access
- Triggers Suricata alerts and endpoint detections.

###  Metasploitable
- Vulnerable Linux target.
- Accepts attacks from Kali Linux.
- Traffic is observed by Suricata.

---

##  Log & Data Flow

```text
[Windows 10 + Elastic Agent] ─────────────┐
                                         │
[Kali Linux] ─┐                          ▼
              │                   [Logstash → Elasticsearch]
              ▼                  ↗
       [Suricata NIDS] → [Filebeat] 
              ▲
[Metasploitable Target]
