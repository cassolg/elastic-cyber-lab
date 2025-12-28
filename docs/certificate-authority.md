# Certificate Authority & TLS Configuration

This document explains how a self-managed Certificate Authority (CA) was created and used to secure communication between Elastic components (Fleet Server, Kibana, Agents) using TLS.

Instead of using Elastic’s built-in TLS auto-configuration, this setup manually generates and manages X.509 certificates to simulate real-world enterprise environments with internal CAs.

---

## Objective

- Create a trusted internal CA using `elasticsearch-certutil`
- Generate and sign certificates for Fleet Server using a **DNS-resolvable domain**
- Secure Fleet Server communication using TLS
- Ensure Elastic Agents and Kibana communicate over HTTPS

---

## Tools Used

- `elasticsearch-certutil` (bundled with Elastic Stack)
- `unzip`, `openssl`, and Linux permissions tools
- Manual cert placement and configuration

---

## Steps Performed



```bash

Step 1: Generate a Certificate Authority (CA)
elasticsearch-certutil ca --pem

Step 2: Unzip and Organize the CA Files
unzip elastic-stack-ca.zip -d certs/ca
Files are organized as:
certs/ca/ca.crt
certs/ca/ca.key

Step 3: Generate a Certificate for Fleet Server (with DNS name)
elasticsearch-certutil cert --name fleet-server.yourdomain.local \
  --ca-cert certs/ca/ca.crt \
  --ca-key certs/ca/ca.key \
  --pem
Note: The --name flag was set to a DNS domain (e.g., fleet-server.lab.local) instead of an IP address. This ensures that TLS validation works when Elastic Agents connect using a hostname.

Step 4: Unzip and Organize Fleet Server Certs
unzip elastic-certificates.p12.zip -d certs/fleet
Resulting files:
certs/fleet/fleet-server.crt
certs/fleet/fleet-server.key

Step 5: Set Correct Permissions
chown -R root:root certs/
chmod -R 600 certs/

```
## Final File Structure
```bash
/certs
  ├── /ca
  │   ├── ca.crt
  │   └── ca.key
  └── /fleet
      ├── fleet-server.crt
      └── fleet-server.key
```
## Integration Point

These certificates will be used in the next phase during Fleet Server installation:
```bash
elastic-agent install \
  --fleet-server-es-ca=/path/to/ca.crt \
  --fleet-server-cert=/path/to/fleet-server.crt \
  --fleet-server-cert-key=/path/to/fleet-server.key \
  --url=https://fleet-server.lab.local:8220
```

## Screenshots

All certificate generation steps are documented under:
```bash
/images/certificate-authority/
```

## Related Docs

Fleet Server Setup

ELK Stack Setup
