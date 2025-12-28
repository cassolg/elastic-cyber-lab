# Fleet Server Setup (with TLS)

This guide documents the deployment of the **Elastic Fleet Server** on Ubuntu, configured to use **TLS encryption with a custom Certificate Authority (CA)** and a DNS-resolvable domain.

Fleet Server acts as the central control plane for managing Elastic Agents in the Elastic Stack. In this setup, we secured Fleet Server using manually generated X.509 certificates and verified secure agent connectivity via Kibana.

---

## Prerequisites

- Elastic Stack installed (Elasticsearch + Kibana)
- Certificate Authority and signed certs ready (see: `certificate-authority.md`)
- DNS resolution working (e.g. `fleet-server.lab.local`)

---

## Tools Used

- Elastic Agent (Linux installer)
- Self-signed TLS certificates (from our custom CA)
- Kibana Fleet UI (to validate connection)

---

## Certificate Structure

Before installing Fleet Server, ensure the following certs exist:
```bash
/certs
├── /ca/ca.crt
└── /fleet
├── fleet-server.crt
└── fleet-server.key
```

The certificate must have been generated using the Fleet Server's **DNS name** (e.g., `fleet-server.lab.local`), not an IP address.

---

## Install Fleet Server with TLS


```bash
Step 1: Download and unpack the Elastic Agent
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.x.x-linux-x86_64.tar.gz
tar -xzf elastic-agent-8.x.x-linux-x86_64.tar.gz
cd elastic-agent-8.x.x-linux-x86_64
chmod +x elastic-agent

Step 2: Run the Fleet Server install command
sudo ./elastic-agent install \
  --fleet-server-es=https://<elasticsearch-ip>:9200 \
  --fleet-server-service-token=<your-token> \
  --fleet-server-policy=<fleet-server-policy-id> \
  --certificate-authorities=/path/to/ca.crt \
  --fleet-server-cert=/path/to/fleet-server.crt \
  --fleet-server-cert-key=/path/to/fleet-server.key \
  --url=https://fleet-server.lab.local:8220
Important:
The --url must match the DNS name used in your certificate.
Use the correct service token generated from the Kibana → Fleet UI.

Step 3: Confirm Fleet Server is running
After successful installation, check service status:
sudo systemctl status elastic-agent
You should see output indicating that the Fleet Server is active.

```
## Verify in Kibana
Open Kibana → Fleet → Fleet Servers
Confirm that your Fleet Server shows up as Healthy
The DNS name (fleet-server.lab.local) should appear in the host column

## Notes
If agents fail to connect, verify that:
The CA is trusted by the agent
The DNS name in the cert matches what you use in --url
Port 8220 is open on the Fleet Server host
You can test TLS manually using:
```bash
curl --cacert /path/to/ca.crt https://fleet-server.lab.local:8220
```
## Screenshots
All installation and verification steps are documented in:
/images/fleet-server/
