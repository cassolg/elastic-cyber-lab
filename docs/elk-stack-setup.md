# ELK Stack Setup (Elasticsearch, Logstash, Kibana)

This document outlines the installation and configuration of the Elastic Stack components (Elasticsearch, Kibana, and Logstash) on Ubuntu Server 22.04. These components form the foundation for centralized log collection, monitoring, and visualization in the lab environment.

---

## Prerequisites

- Ubuntu 24.04 LTS (minimal install)
- Internet access to pull packages from Elastic’s APT repository
- Sudo/root privileges

---

## Tools Installed

- Elasticsearch: stores and indexes data
- Kibana: provides a GUI for data visualization and agent management
- Filebeat Lightweight shipper to forward logs to Elasticsearch or Logstash
- Logstash : used for data processing pipelines

---

## Installation Steps


```bash
1. Update system packages:
sudo apt update && sudo apt upgrade -y

2.Add the Elastic APT repository and GPG key:

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update

3. Install Elasticsearch:

sudo apt install elasticsearch -y

4. Enable and start Elasticsearch:

sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

5. Verify that Elasticsearch is running:

curl -X GET http://localhost:9200

6. Install Kibana:

sudo apt install kibana -y

7. Enable and start Kibana:

sudo systemctl enable kibana
sudo systemctl start kibana

8. Install Logstash:

sudo apt install logstash -y

9. Install and Configure Filebeat

sudo apt install filebeat -y


10. Enable system module:

sudo filebeat modules enable system


11. Set up Filebeat (dashboards, ingest pipelines):

sudo filebeat setup


12. Start and enable the Filebeat service:

sudo systemctl enable filebeat
sudo systemctl start filebeat
```
## Status Check

Use **systemctl status** elasticsearch, kibana, filebeat, and logstash to verify services are active.

Access Kibana in a browser at http://<your-server-ip>:5601

## Screenshots

All screenshots for this setup are located in:
/images/elk-stack-setup/

