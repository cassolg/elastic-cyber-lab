#  Elasticsearch Configuration (`elasticsearch.yml`)

>  **Location:** `elastic-setup/elasticsearch/elasticsearch.yml`  
>  This file configures the core behavior, security, and network settings of your Elasticsearch node in the Elastic Cyber Lab.

This guide explains the key configuration options used in the lab setup. The focus is on enabling secure communications, authentication, and basic lab-ready performance settings.

---

##  1. Data & Log Paths

```yaml
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
```
path.data: Stores indexed data.

path.logs: Stores Elasticsearch service logs.

## 2. Memory Locking

```yaml
bootstrap.memory_lock: true
```

Prevents memory from being swapped to disk.

Helps avoid performance issues with the JVM.

Make sure your systemd service has:
LimitMEMLOCK=infinity

## 3. Network Configuration

```yaml
network.host: 0.0.0.0
# http.port: 9200
```

Binds Elasticsearch to all interfaces

This allows remote VMs (e.g., Kibana, Filebeat, Suricata) to connect.

The default HTTP port is 9200 — can be explicitly set if needed.

## 4. Security (X-Pack)

```yaml
xpack.security.enabled: true
```

Enables authentication, role-based access control, and TLS.

Required to securely interact with Kibana, Filebeat, Logstash, etc.

## 5. TLS for HTTP (Client Access)

```yaml
xpack.security.http.ssl:
  enabled: true
  key: certs/manual/es01/es01.key
  certificate: certs/manual/es01/es01.crt
  certificate_authorities: [ certs/manual/ca/ca.crt ]
```

Encrypts HTTP API communication (e.g., with Kibana, Elastic Agent).

Uses a manually generated certificate and CA.

All clients must trust this CA to connect.

## 6. TLS for Transport Layer (Node-to-Node)

```yaml
xpack.security.transport.ssl:
  enabled: true
  verification_mode: certificate
  key: certs/manual/es01/es01.key
  certificate: certs/manual/es01/es01.crt
  certificate_authorities: [ certs/manual/ca/ca.crt ]
```

Even in single-node mode, transport TLS is required when security is enabled.

Same certs as HTTP are used here for simplicity in lab.

Verifies identity between nodes using certificates.

## 7. Optional: Single Node Discovery

```yaml
# discovery.type: single-node
```

Uncomment this if you're running a single-node cluster, which you most likely are in a lab setup.

Prevents cluster formation errors.

Avoids bootstrap checks meant for multi-node deployments.

### For more details
Go to "images/elk-stack-setup/kibana"
