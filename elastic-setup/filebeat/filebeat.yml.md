#  Filebeat Configuration (`filebeat.yml`)

>  **Path:** `/etc/filebeat/filebeat.yml`  
>  This Filebeat config is used in the Elastic Cyber Lab to collect Apache and system logs and forward them to Logstash.

This document breaks down the actual `filebeat.yml` configuration used in the lab environment. It sends logs to Logstash over port `5044` for further processing and ingestion into Elasticsearch.

---

##  1. Filebeat Inputs

```yaml
filebeat.inputs:
  - type: filestream
    id: my-filestream-id
    enabled: true
    paths:
      - /var/log/*.log
      - /var/log/apache2/*.log
```
Input Type: filestream (for reading log files from disk)

ID: my-filestream-id (unique identifier)

Paths:

Collects all log files from /var/log/

Also monitors Apache logs specifically in /var/log/apache2/

<img width="942" height="501" alt="filebeat inputs" src="https://github.com/user-attachments/assets/893c6856-7e2e-45fe-a8e3-1b8343895334" />

---

## 2. Output to Logstash
```yaml
output.logstash:
  hosts: ["10.XX.X.XXX:5044"]
```

Filebeat forwards all collected logs to Logstash on port 5044

This matches the Logstash input config on the same port

<img width="879" height="510" alt="filebeat logstash output" src="https://github.com/user-attachments/assets/22a9ad1d-dd20-49df-b5d3-4b96efedd4bc" />


 SSL is not enabled here, which is fine for isolated lab use. (The SSL config is commented out.)

--- 

## 3. Processors
```yaml
processors:
  - add_host_metadata:
      when.not.contains.tags: forwarded
  - add_cloud_metadata: ~
  - add_docker_metadata: ~
  - add_kubernetes_metadata: ~
```

These processors enrich logs with metadata:

add_host_metadata: Adds OS, hostname, etc.

Others are enabled but do nothing unless you're running in cloud, Docker, or K8s (safe to leave in place)

---

## 4. Filebeat Modules
```yaml
filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false
```

This enables Filebeat module configuration from the modules.d/ folder.

Reload is disabled, so you must restart Filebeat to apply module changes.

<img width="860" height="434" alt="filebeat modules" src="https://github.com/user-attachments/assets/72d5f3b5-968b-49b6-a69e-4a6a1544c61f" />

---

## 5. Kibana Setup (Optional)
```yaml
setup.kibana:
# host: "localhost:5601"
```
This section allows Filebeat to load dashboards into Kibana using the Kibana API.

In this configuration, it's currently commented out, meaning Filebeat will not attempt to connect to Kibana directly.

This is appropriate for this current setup, where Filebeat sends logs to Logstash, not directly to Elasticsearch/Kibana.

<img width="1007" height="385" alt="filebeat kibana" src="https://github.com/user-attachments/assets/98c8841f-2a22-484d-a6b8-a36cd019402d" />



