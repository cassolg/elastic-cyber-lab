# Nmap Detection Test (Proof of IDS Functionality)

---

## Objective

To verify that Suricata successfully detected active network reconnaissance performed from the attacker machine.

## Test Scenario

A simulated attacker (Kali Linux) performed an Nmap service and version scan against the Metasploitable target machine inside the isolated lab network.

### Attack command executed:
```linux
nmap -sS -sV -O 192.XXX.XX.XX
```
This scan attempted to:
```text
Discover open ports

Identify running services

Fingerprint the operating system
```

## Network Flow

```text
Kali Linux (Attacker)
        ↓
Internal Network (Attack_net)
        ↓
Metasploitable (Target)
        ↓
Suricata IDS (Packet Inspection)
        ↓
Filebeat (Log Shipping)
        ↓
Elasticsearch (Indexing)
        ↓
Kibana SIEM (Analysis)
```
---
## Evidence of Detection

### Suricata Captured Malicious Traffic

Suricata inspected live network packets and logged suspicious scanning behavior in eve.json.

Captured indicators included:

Multiple connection attempts to different ports

Service enumeration patterns

Known Nmap scripting engine signatures

### Elasticsearch Indexed the Events

The Suricata logs were successfully shipped via Filebeat and indexed into Elasticsearch under:
```linux
filebeat-*
```
---

### Kibana Revealed Nmap Fingerprints

Filtering for relevant events exposed clear reconnaissance indicators:

Suspicious HTTP requests

Nmap scripting engine user-agent

Unusual port scanning patterns

Example indicator:

```text
Nmap Scripting Engine https://nmap.org/book/nse.html
```

This confirmed that attacker activity was fully visible to the SIEM.

## Validation Screenshots

The following evidence confirms successful detection:

### Nmap Scan Results (Attacker Perspective)

Shows successful service enumeration of the target.

### Suricata Network Interface Monitoring

Confirms IDS visibility into attack network traffic.

---

### Kibana SIEM Logs

Displays indexed events containing Nmap signatures and reconnaissance behavior.

---

### Result

The test successfully demonstrated that:

 Suricata detected live reconnaissance traffic
 Filebeat shipped logs without parsing errors
 Elasticsearch indexed security events correctly
 Kibana enabled investigation of attacker behavior

The IDS pipeline operated end-to-end without packet loss or ingestion failures.

<img width="722" height="503" alt="metasploitable network setup" src="https://github.com/user-attachments/assets/7569ebe7-798c-4ee7-8d67-0a51c12e3738" />
<img width="642" height="500" alt="kali ping" src="https://github.com/user-attachments/assets/3a4c112a-9074-4d48-a6c8-597ceaecb4ca" />
<img width="652" height="516" alt="nmap result" src="https://github.com/user-attachments/assets/063a5d74-b745-4a05-aee3-1e15664caf96" />
<img width="1238" height="679" alt="detection nmap" src="https://github.com/user-attachments/assets/e3630469-42c4-49eb-b28b-6159b26d611d" />






