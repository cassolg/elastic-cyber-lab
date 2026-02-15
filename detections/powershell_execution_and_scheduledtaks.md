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

An alert was generated after detecting a PowerShell command executed with encoded parameters.
After decoding the command, it was confirmed the script was executed directly in memory, which is a common attacker technique to avoid writing files to disk.

Shortly after, a scheduled task named UpdaterService was created to run the same command at logon, indicating persistence.

### Investigation

Reviewed Sysmon process logs in Elastic

Decoded the Base64 command using CyberChef

Confirmed in-memory execution

Located the scheduled task responsible for persistence

Checked the system for additional suspicious tasks

### Remediation

Removed the malicious scheduled task

Verified no other persistence remained

Enabled PowerShell script block logging for better visibility

### Result

No further malicious activity was observed after removal.
The system remained stable and monitoring continued.

This incident represents a typical fileless persistence attempt using encoded PowerShell and scheduled task execution.

---

## Skills Demonstrated

### Threat Detection & Analysis

Investigated Sysmon process creation events in Elastic SIEM

Identified suspicious PowerShell execution using encoded commands

Correlated multiple events to detect persistence behavior

### Incident Response

Analyzed attacker behavior and confirmed fileless execution technique

Decoded Base64 payload using CyberChef

Documented findings and updated incident case notes

### Threat Hunting

Created custom KQL detection rules for PowerShell abuse

Built detection for scheduled task persistence

Validated detections through attack simulation

### System Hardening

Removed malicious scheduled task persistence

Enabled PowerShell Script Block Logging

Verified no additional persistence mechanisms remained

### SIEM Engineering

Tuned Elastic detection rules to reduce false negatives

Generated alerts and attached them to investigation cases

Performed log-driven validation of detections

---

<img width="1153" height="575" alt="Powershell_ScheduledTaskCase" src="https://github.com/user-attachments/assets/b98d009a-1949-4ff3-ba0f-e7526baed3fe" />
<img width="1184" height="368" alt="Powershell_ScheduledTaskCase (2)" src="https://github.com/user-attachments/assets/f4459a1b-1e7e-4931-bb8b-50b5fa891483" />
<img width="1189" height="68" alt="Powershell_ScheduledTaskCase (3)" src="https://github.com/user-attachments/assets/7b77f051-ce95-4aea-b935-34d459a8cab8" />
<img width="716" height="616" alt="Powershell_ScheduledTaskCase (4)" src="https://github.com/user-attachments/assets/9db4b52a-2394-4d1e-a602-ac7e39ce8eaf" />
<img width="707" height="595" alt="Powershell_ScheduledTaskCase (5)" src="https://github.com/user-attachments/assets/ce26758a-5069-4da2-8b11-2a4ee1fe840d" />
<img width="717" height="677" alt="Powershell_ScheduledTaskCase (6)" src="https://github.com/user-attachments/assets/ad7dfa68-c9cf-48a5-b8f7-5ab21194df77" />
<img width="713" height="600" alt="Powershell_ScheduledTaskCase (7)" src="https://github.com/user-attachments/assets/08f05b3b-6565-4df1-8741-2a12bcbfc3f7" />
<img width="1185" height="226" alt="Powershell_ScheduledTaskCase (8)" src="https://github.com/user-attachments/assets/1c512967-9cf7-4014-9aeb-75cd31b5b561" />
<img width="1904" height="627" alt="Captura de tela 2026-02-15 131708" src="https://github.com/user-attachments/assets/8855a6b1-4580-4d3e-a052-5f75979bdf00" />
<img width="776" height="396" alt="Captura de tela 2026-02-15 131232" src="https://github.com/user-attachments/assets/22fc2ac7-e881-4ec9-af1e-d220e63f6848" />
<img width="855" height="161" alt="Captura de tela 2026-02-15 130716" src="https://github.com/user-attachments/assets/bd59ec9c-eb2a-4a51-8aaa-5112ee016ad3" />
<img width="841" height="149" alt="Captura de tela 2026-02-15 130846" src="https://github.com/user-attachments/assets/427b1f89-35ab-4b36-806c-66c4a26da71f" />
<img width="823" height="580" alt="Captura de tela 2026-02-15 131058" src="https://github.com/user-attachments/assets/2f17e8bb-5294-421f-96e2-421543b46b83" />
<img width="812" height="493" alt="Captura de tela 2026-02-15 131606" src="https://github.com/user-attachments/assets/2024f82b-1705-4b2c-b233-54eccadd0727" />
<img width="625" height="466" alt="Captura de tela 2026-02-15 131400" src="https://github.com/user-attachments/assets/8df84381-f6c4-4dd4-8416-67410eac71fb" />
<img width="496" height="111" alt="Captura de tela 2026-02-15 131421" src="https://github.com/user-attachments/assets/f01843b5-6b68-4063-ac32-6f8c9b2844be" />
<img width="564" height="74" alt="Captura de tela 2026-02-15 131529" src="https://github.com/user-attachments/assets/902d516e-4bd9-4e21-aa79-9b2d01a03327" />
<img width="965" height="145" alt="Captura de tela 2026-02-15 131633" src="https://github.com/user-attachments/assets/cfee864d-3f5d-40d4-8d67-da211911c383" />






