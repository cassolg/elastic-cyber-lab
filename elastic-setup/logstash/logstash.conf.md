#  Logstash Configuration (`logstash.conf` / `beats.conf`)

>  **Path:** `elastic-setup/logstash/`  
>  **Config file:** `/etc/logstash/conf.d/beats.conf`  

This document explains how Logstash is configured in the Elastic Cyber Lab to receive logs from Beats, parse different log types (Apache and Syslog), and forward them securely to Elasticsearch.

---

##  1. Input Configuration (Beats)

```conf
input {
  beats {
    port => 5044
  }
}
```
Logstash listens on port 5044 for incoming data from Elastic Agent or Filebeat.

This port is open on the VM running Logstash.
<img width="789" height="143" alt="beats conf" src="https://github.com/user-attachments/assets/43dcf623-b364-4641-a3bc-fc67d814680d" />


## 2. Filter Section

Logstash parses multiple types of logs using conditional logic.

### System Logs
```conf
if "system" in [fileset][module] {
  mutate { add_field => { "type" => "syslog" } }

  grok {
    match => { "message" => "%{SYSLOGLINE}" }
  }

  date {
    match => [ "timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
  }
}
```

<img width="789" height="278" alt="conf d system logs" src="https://github.com/user-attachments/assets/36661e2a-0a6e-41e2-a20d-0cf1b738070d" />


Matches system logs from the Beats system module.

Uses grok to extract fields from standard syslog lines.

Uses date to normalize timestamps.

### Apache Logs
```conf
if "apache" in [fileset][module] or "access" in [fileset][name] {
  mutate { add_field => { "type" => "apache" } }

  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }

  date {
    match => [ "timestamp", "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}
```

Matches Apache logs using the combined log format.

Extracts common fields: client IP, method, URI, status, etc.

Timestamp is parsed in Apache log format.

<img width="754" height="309" alt="conf d apache logs" src="https://github.com/user-attachments/assets/4216eb5f-a051-4740-a110-faf74c16351f" />




## 3. Output to Elasticsearch (TLS Enabled)
```conf
output {
  elasticsearch {
    hosts => ["https://10.X.X.XXX:9200"]
    user => "elastic"
    password => "your_password_here"
    cacert => "/etc/logstash/certs/http_ca.crt"
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
  }
}
```
<img width="526" height="200" alt="conf-file-output-logstash" src="https://github.com/user-attachments/assets/c458c1d3-1ff8-4aec-9500-18529521f12b" />


Logstash sends data to Elasticsearch over HTTPS.

Uses the elastic superuser with hardcoded credentials (lab setup).

TLS certificate is specified via cacert.

Output index is dynamically named based on the Beat type and date.


## 4. Logstash Service Status
```bash
sudo systemctl status logstash
```

Logstash is installed as a systemd service.

Status shows it's running, memory usage, and PID.

Starts automatically on boot.

<img width="1793" height="306" alt="logstash-running" src="https://github.com/user-attachments/assets/8346df45-d8da-45aa-b1db-c8cdbcdc41a8" />
