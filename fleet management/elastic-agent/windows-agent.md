#  Elastic Agent on Windows 10

>This document outlines how the **Elastic Agent was installed on a Windows 10 VM** as part of the Elastic Cyber Lab.



---

##  Purpose

- To forward logs from a Windows 10 endpoint to the ELK stack
- Use **Elastic Agent** to collect data via **Fleet**
- Apply a **Windows Endpoint Policy** from Kibana
- Ingest logs (e.g. Sysmon) and send them to Elasticsearch

---

##  Setup Summary

| Component      | Value                           |
|----------------|---------------------------------|
| OS             | Windows 10 VM                   |
| Agent Type     | Elastic Agent                   |
| Management     | Fleet Server (running on Ubuntu)|
| Assigned Policy| `windows-endpoint-policy`       |
| Agent Version  | 8.19.9                          |
| Communication  | Enrolled via port 8220 (Fleet)  |

---

##  Installation Steps

The Elastic Agent was downloaded from Kibana:

Fleet → Agents → Add Agent → Windows → Use Elastic Agent with Fleet


The provided PowerShell install command was run on the Windows VM:

```powershell
Start-Process powershell -Verb runAs

.\elastic-agent.exe install `
  --url=https://es01.soc.lab:8220 `
  --enrollment-token=<ENROLLMENT_TOKEN>
```

es01.soc.lab points to the Ubuntu Fleet Server

The enrollment token was generated during setup in Kibana

Once installed, the agent runs as a Windows service.

## Post-Install Verification

The agent appeared in Fleet → Agents with:
```doc
Field --->	Value
Host	DESKTOP-D6MDETS
Agent Policy --->	windows-endpoint-policy
Status --->	 Healthy
Last Activity --->	Real-time updating
Version --->	8.19.9
```
---

### Logs and Indices

Under Stack Management → Index Management, you could see:

New logs-* indices

Some filebeat-* indices (if Filebeat modules used previously)

These confirm successful log ingestion from the Windows endpoint.

---

### Ingested Data (Sysmon)

Sysmon logs were being ingested through the Elastic Agent integration.
The integration was assigned via the policy windows-endpoint-policy, which included:

Windows integration

System logs

Event logs (Sysmon XML installed manually earlier)

Sysmon configuration:

Installed via .exe on Windows

Config used: sysmon-config.xml in windows-setup/sysmon/

---

## Additional Notes

Hostname in Fleet: DESKTOP-D6MDETS

Agent is shown as Healthy

No certificate issues were encountered

Windows Agent was managed entirely from the Fleet UI

Policy updates applied without needing re-install

## Troubleshooting Steps (If Needed)

Check service status:
```powershell
Get-Service -Name elastic-agent
```

Log location on Windows:
```powershell

C:\Program Files\Elastic\Agent\data\elastic-agent-*\logs\
```

Force unenroll if needed:
```powershell

.\elastic-agent.exe uninstall

```

## Outcome

The Windows 10 endpoint is:

Fully integrated into Fleet

Running Elastic Agent

Shipping logs to Elasticsearch

Policy managed through Kibana
