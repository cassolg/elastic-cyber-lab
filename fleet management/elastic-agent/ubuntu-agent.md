#  Elastic Agent on Ubuntu

>This document covers how the **Elastic Agent was installed on the Ubuntu VM** to collect system logs and send them to Fleet Server.

This is the same host that runs:
- Elasticsearch
- Kibana
- Logstash
- Fleet Server



---

##  Purpose

The goal was to:
- Collect system-level logs from the Ubuntu host
- Use the **Elastic Agent** to ship logs via Fleet
- Assign a policy (e.g. `system-1`) through Kibana
- Validate end-to-end pipeline from agent → Fleet → Elasticsearch → Kibana

---

##  Lab Context

- **Host OS:** Ubuntu
- **Hostname(s):** `es01.soc.lab` / `labtest-VirtualBox`
- **Agent Role:** Acts as both Fleet Server **and** system log forwarder
- **Policy Assigned:** `Fleet Server Policy` (includes system integration)

---

##  Installation Steps

> The agent was installed using the same **Fleet → Add agent** flow in Kibana.

Command used:

```bash
sudo ./elastic-agent install \
  --url=https://es01.soc.lab:8220 \
  --enrollment-token=<ENROLLMENT_TOKEN>
```
Where:

--url: Fleet Server URL on port 8220

--enrollment-token: Copy/pasted from Kibana during agent enrollment

No manual config or YAML editing was done

---

## Outcome in Kibana

In Fleet → Agents, the Ubuntu agent appeared with:

```doc
Field ---->	Value
Host ---->	labtest-VirtualBox
Agent Policy ---->	Fleet Server Policy
Status ---->	 Healthy
Version ---->	8.19.8
Platform ---->	Ubuntu
Logging Level ---->	Info
```
---

## Integrations Applied


The Fleet Server Policy included:
```doc
 fleet_server-1

 system-1
```

This means the same host runs:

Fleet Server

Elastic Agent (as system log forwarder)

---

## Logs and Agent Info

To verify agent status:

```bash
sudo systemctl status elastic-agent
```

Expected output:
```bash
● elastic-agent.service - Elastic Agent
   Active: active (running)
```

Agent logs are stored at:
```bash
/var/log/elastic-agent/
```

Agent binaries are located at:
```bash
/opt/Elastic/Agent
```
---

## Testing and Verification

Logs ingested from the Ubuntu Elastic Agent appeared under:

filebeat-* indices (visible in Stack Management → Index Management)

Dashboards tied to system integration
