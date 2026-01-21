# ELK Stack Installation (Ubuntu – VirtualBox)

This guide documents the manual installation and configuration of the Elastic Stack (Elasticsearch, Logstash, Kibana, Filebeat) on an Ubuntu virtual machine.

The stack serves as the **central logging and detection server** for the lab environment.

---

##  Environment

- **Host Platform:** Oracle VirtualBox
- **Operating System:** Ubuntu Server 22.04 LTS
- **Lab Purpose:** Cybersecurity log aggregation and threat detection

---

##  Components Installed

| Component | Purpose |
|----------|--------|
| **Elasticsearch** | Stores and indexes logs |
| **Kibana** | Visualizes logs and detections |
| **Logstash** | Parses and processes logs |
| **Filebeat** | Ships logs from Suricata and endpoints |

---

##  Prerequisites

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl gnupg apt-transport-https ca-certificates -y
```

## Add Elastic Repository
```bash
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

sudo apt update
```

## Install Elastic Stack
```bash
sudo apt install elasticsearch kibana logstash filebeat -y
```

## Configure Elasticsearch
```bash
Edit the config file:

sudo nano /etc/elasticsearch/elasticsearch.yml
```

## Start Elasticsearch:
```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

## Set Elastic Password
```bash
After startup, run:

sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
```

## Configure Kibana
```bash
Edit:

sudo nano /etc/kibana/kibana.yml
```

## Start Kibana:
```bash
sudo systemctl enable kibana
sudo systemctl start kibana
```

## Access Kibana from your host browser:
```bash
http://<Ubuntu_VM_IP>:5601
```

## Configure Logstash
```bash
Create pipeline:

sudo nano /etc/logstash/conf.d/logstash.conf
```
## Start Logstash:
```bash
sudo systemctl enable logstash
sudo systemctl start logstash
```

## Configure Filebeat
```bash
Edit:

sudo nano /etc/filebeat/filebeat.yml
```

## Start Filebeat
```bash
sudo systemctl start filebeat
```

## Verify Services
```bash
sudo systemctl status elasticsearch
sudo systemctl status kibana
sudo systemctl status logstash
sudo systemctl status filebeat
```

## Test Elasticsearch
```bash
curl -u elastic:<password> http://localhost:9200
```
