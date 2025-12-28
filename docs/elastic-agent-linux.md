# Elastic Agent Setup (Linux)

This guide explains how to install and enroll the Elastic Agent on a **Linux endpoint**, using a secure connection to the Fleet Server over **TLS**. The agent will stream system logs and metrics to Elasticsearch via the Fleet Server.

This setup assumes that Fleet Server is already running and secured using your custom TLS certificates and internal Certificate Authority (CA).

---

## Prerequisites

- Linux VM or host (e.g., Ubuntu 22.04)
- Fleet Server running with a valid TLS cert
- Trusted CA (same one used to generate Fleet Server cert)
- Enrollment token generated from Kibana → Fleet UI
- DNS resolution (e.g., `fleet-server.lab.local`)

---

## Tools Used

- Elastic Agent binary (Linux tarball)
- TLS certificate files (CA + cert chain)
- Kibana Fleet UI (for verification)

---

## Installation Steps


```bash
Step 1: Download and Unpack Elastic Agent

curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.x.x-linux-x86_64.tar.gz
tar -xzf elastic-agent-8.x.x-linux-x86_64.tar.gz
cd elastic-agent-8.x.x-linux-x86_64
chmod +x elastic-agent

Step 2: Trust the CA Certificate

Ensure the Linux system trusts the internal CA that signed your Fleet Server cert.
sudo cp /path/to/ca.crt /usr/local/share/ca-certificates/ca.crt
sudo update-ca-certificates

Step 3: Enroll and Install Elastic Agent

Use the install command to securely connect to Fleet Server using TLS:
sudo ./elastic-agent install \
  --url=https://fleet-server.lab.local:8220 \
  --enrollment-token=<your-token> \
  --certificate-authorities=/path/to/ca.crt

Step 4: Verify the Agent is Running

After installation:
sudo systemctl status elastic-agent
```

## Verify in Kibana
```bash
Open Kibana → Fleet → Agents and confirm:
The Linux host appears in the agent list
Status is Healthy
Integration (e.g., System) is enabled and streaming logs
```
## Notes
```bash
Ensure the hostname in --url matches the DNS name on the Fleet Server’s certificate.
Port 8220 must be accessible between the agent and the Fleet Server.
You can enroll multiple Linux endpoints using the same method and token (if reusable).
```
### Screenshots
```bash
All images for this setup are stored in:
/images/elastic-agent-linux/
```
## Related Docs
```bash
Fleet Server Setup
Certificate Authority & TLS
Elastic Agent - Windows
```
