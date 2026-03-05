# Suricata Integration with Elastic Stack — Elastic Cyber Lab

## Objective

After configuring Suricata as a functioning IDS sensor, I integrated it with the Elastic Stack to centralize network telemetry for analysis, visualization, and detection engineering.

The goal was to ensure Suricata alerts and protocol logs could be ingested, parsed, indexed, and investigated within the SIEM platform.

---

## Log Forwarding Pipeline

To transport Suricata telemetry to Elasticsearch, I deployed Filebeat as a lightweight log shipper on the Suricata host.

Filebeat was configured to:
```text
Monitor Suricata EVE JSON logs

Parse structured network events

Enrich metadata fields

Forward events securely to Elasticsearch
```
This enabled automated ingestion of network security data into the SIEM.

---

## Suricata Module Enablement

I enabled the native Filebeat Suricata module, which provided built-in parsing pipelines for:
```text
Alert events

Flow records

DNS logs

HTTP transactions

TLS metadata

SSH activity
```
Using the module avoided the need for custom parsing rules and ensured ECS-compatible field mapping.

---

## Secure Elasticsearch Connection

Filebeat was configured to connect securely to the Elastic node using authenticated HTTPS communication.

This ensured:
```text
Encrypted log transport

Proper authentication

Reliable indexing

Compatibility with Elastic security defaults
```
---

## Data Ingestion & Indexing

Once enabled, Suricata events were successfully indexed into Elasticsearch.

Network telemetry became searchable and filterable by:
```text
Source and destination IPs

Network ports

Protocol types

Alert signatures

Event categories

Timestamps
```
This provided full visibility into network activity across the lab environment.

---

## Visualization in Kibana

Using Kibana, I confirmed successful ingestion by:

Querying Suricata indices in Discover

Reviewing parsed alert fields

Inspecting protocol metadata

Filtering events by attacker and target systems

Prebuilt dashboards further enabled visualization of:
```text
Network traffic volume

Protocol distribution

Alert trends

Top talkers

Suspicious activity patterns
```
---

## Operational Impact

Integrating Suricata with Elastic transformed raw packet inspection into actionable security intelligence.

This allowed:
```text
Centralized monitoring

Cross-source correlation

Threat hunting workflows

Alert development

Case investigation readiness
```
The SOC lab now had both endpoint and network telemetry feeding a unified analysis platform.

---

## Outcome

The integration successfully established an end-to-end pipeline from:

Network Traffic → IDS Detection → Log Shipping → SIEM Indexing → Investigation

This completed the network telemetry ingestion layer of the Elastic Cyber Lab and prepared the environment for detection engineering and alert creation.

<img width="796" height="501" alt="filebeat yaml suricata" src="https://github.com/user-attachments/assets/a974aa91-471a-4fb7-bf9c-4c2153d61ea6" />
<img width="974" height="776" alt="filebeat suricata" src="https://github.com/user-attachments/assets/1a4b58ea-6841-4b8b-b53e-c7f50e22d8e5" />
