# SOC Case Study — Encoded PowerShell Execution & Scheduled Task Persistence Overview
---
> This project simulates a real-world post-exploitation scenario inside a home SOC lab using the Elastic Stack. The objective was to detect attacker behavior rather than simply collect logs. Custom detection rules were written to identify command obfuscation and persistence techniques, then investigated and documented as an incident case.

---

## Lab Environment

Windows 10 Endpoint with Sysmon

Elastic Stack (Elasticsearch, Kibana, Fleet, Elastic Agent)

Filebeat Windows integration

VirtualBox segmented lab network

---

## Data Source:

Sysmon Operational Logs (Event ID 1 — Process Creation)

---

## Attack Simulation
Stage 1 — Obfuscated Command Execution

A PowerShell command was executed using Base64 encoded arguments to simulate attacker payload execution.

Example behavior:
```powershell
powershell.exe -enc <base64 payload>
```

Purpose: Attackers commonly encode commands to evade detection and bypass basic security controls.

---

## Stage 2 — Persistence Establishment

A scheduled task was created to execute a command at user logon.

Example behavior:
```powershell
schtasks /create /sc onlogon /tn UpdaterService /tr <command> /ru user
```

Purpose: Provides automatic re-execution of payload after reboot or user login.

---

## Detection Engineering
Rule 1 — Encoded PowerShell Execution

Detects suspicious PowerShell using encoded or obfuscated arguments.

### Logic

Sysmon Event ID 1
```powershell
process.name = powershell.exe
```

Arguments contain encoding indicators

Security Value Indicates potential payload delivery, loaders, or in‑memory execution.

### MITRE ATT&CK:

T1059.001 — PowerShell

T1027 — Obfuscated/Encoded Commands

---

### Rule 2 — Scheduled Task Persistence

Detects creation of a scheduled task by a non‑SYSTEM user.

### Logic

Sysmon Event ID 1
```powershell
process.name = schtasks.exe
```
Arguments contain /create

Not executed by SYSTEM

Security Value Identifies persistence mechanism used after initial compromise.

### MITRE ATT&CK:

T1053.005 — Scheduled Task/Job Persistence

---

## Investigation Findings

Two alerts occurred in sequence:

Encoded PowerShell execution

Scheduled task creation at logon

This behavior chain indicates post‑exploitation activity where an attacker first executes a payload and then establishes persistence to maintain access.

---

## Incident Conclusion

The activity represents a likely compromise attempt involving:

Obfuscated command execution

Persistence installation

The system should be considered potentially compromised and require containment and credential review in a production environment.

---

## Skills Demonstrated

Detection engineering (KQL rule creation)

Windows event analysis (Sysmon)

MITRE ATT&CK mapping

Incident documentation

SIEM alert triage workflow


<img width="1153" height="575" alt="Powershell_ScheduledTaskCase" src="https://github.com/user-attachments/assets/b98d009a-1949-4ff3-ba0f-e7526baed3fe" />
<img width="1184" height="368" alt="Powershell_ScheduledTaskCase (2)" src="https://github.com/user-attachments/assets/f4459a1b-1e7e-4931-bb8b-50b5fa891483" />
<img width="1189" height="68" alt="Powershell_ScheduledTaskCase (3)" src="https://github.com/user-attachments/assets/7b77f051-ce95-4aea-b935-34d459a8cab8" />
<img width="716" height="616" alt="Powershell_ScheduledTaskCase (4)" src="https://github.com/user-attachments/assets/9db4b52a-2394-4d1e-a602-ac7e39ce8eaf" />
<img width="707" height="595" alt="Powershell_ScheduledTaskCase (5)" src="https://github.com/user-attachments/assets/ce26758a-5069-4da2-8b11-2a4ee1fe840d" />
<img width="717" height="677" alt="Powershell_ScheduledTaskCase (6)" src="https://github.com/user-attachments/assets/ad7dfa68-c9cf-48a5-b8f7-5ab21194df77" />
<img width="713" height="600" alt="Powershell_ScheduledTaskCase (7)" src="https://github.com/user-attachments/assets/08f05b3b-6565-4df1-8741-2a12bcbfc3f7" />
<img width="1185" height="226" alt="Powershell_ScheduledTaskCase (8)" src="https://github.com/user-attachments/assets/1c512967-9cf7-4014-9aeb-75cd31b5b561" />

