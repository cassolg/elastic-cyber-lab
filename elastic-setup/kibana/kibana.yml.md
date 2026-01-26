#  Kibana Configuration (`kibana.yml`)

>  **Path:** `elastic-setup/kibana/kibana.yml`  
>  This configuration file defines how Kibana is served in the Elastic Cyber Lab environment, including its domain, port, and connection to the secured Elasticsearch instance.

---

##  1. Server Configuration

```yaml
server.port: 5601
server.host: "kibana.soc.lab"
server.publicBaseUrl: "https://kibana.soc.lab:5601"
```
server.port: Kibana is set to listen on port 5601 (default).

server.host: Set to a custom hostname kibana.soc.lab, allowing domain-style access within the lab network.

server.publicBaseUrl: Defines the external URL users will use to access Kibana. This matches the custom domain.

### Screenshot:
<img width="1135" height="642" alt="kibanayaml2" src="https://github.com/user-attachments/assets/4d711a5a-f4f7-4e22-b801-1948472e1600" />



 The domain kibana.soc.lab is added to the /etc/hosts file across VMs to resolve internally.

## 2. Elasticsearch Connection

```yaml
elasticsearch.hosts: ["https://es01.soc.lab:9200"]
elasticsearch.username: "kibana_system"
elasticsearch.password: "Sle3mt4kK20G7Nsrg=k6"
```

elasticsearch.hosts: Points Kibana to a secured Elasticsearch node (https://es01.soc.lab:9200) using the custom internal domain.

username / password: Uses the built-in kibana_system user to authenticate against Elasticsearch.

### Screenshot:
<img width="1139" height="347" alt="kibanayaml" src="https://github.com/user-attachments/assets/b0838776-05df-4e8e-817f-4c8578361c2b" />



## Notes on DNS

To make hostnames like kibana.soc.lab and es01.soc.lab work in the lab:
```bash
/etc/hosts
```
Entries are added on all VMs:
```bash
192.XXX.XX.XX kibana.soc.lab
192.XXX.XX.XX es01.soc.lab
```

This setup mimics internal DNS resolution in enterprise networks.
