# Assignment 4: Log Analysis and Attack Detection

![SIEM](https://img.shields.io/badge/SIEM-ELK%20Stack-blue)
![Attack](https://img.shields.io/badge/Attack-SSH%20Brute%20Force-red)
![Detection](https://img.shields.io/badge/Detection-Kibana-green)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-orange)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

## Overview

This project demonstrates deploying a SIEM using the ELK Stack, simulating an SSH brute force attack using Hydra, and detecting and analysing the attack using Kibana. The lab maps findings to the MITRE ATT&CK framework and applies the NIST Incident Response process.

---

## Lab Architecture

```
┌─────────────────────┐         ┌─────────────────────┐
│   Kali Linux        │         │   Ubuntu 24.04       │
│   172.20.10.7       │────────▶│   172.20.10.2        │
│                     │  SSH    │                      │
│   - Hydra Attack    │  :22    │   - Elasticsearch    │
│   - Kibana Analyst  │         │   - Kibana           │
│     Workstation     │◀────────│   - Filebeat         │
│                     │  :5601  │   - SSH Target       │
└─────────────────────┘         └─────────────────────┘
```

---

## Stack Details

| Component | Version | Purpose |
|-----------|---------|---------|
| Elasticsearch | 8.17.12 | Log storage and indexing |
| Kibana | 8.17.12 | Visualisation and analysis |
| Filebeat | 8.17.12 | Log shipping agent |
| Java | OpenJDK 17.0.18 | Elasticsearch runtime |

---

## Part 1 — ELK Stack Deployment

### Install Elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update && sudo apt install elasticsearch -y
```

### Tune Heap Size (Lab Environment)

```bash
sudo nano /etc/elasticsearch/jvm.options.d/heap.options
```

```
-Xms512m
-Xmx512m
```

### Disable SSL for Lab

Edit `/etc/elasticsearch/elasticsearch.yml`:

```yaml
xpack.security.enabled: false
xpack.security.http.ssl:
  enabled: false
network.host: localhost
http.port: 9200
```

### Install Kibana

```bash
sudo apt install kibana -y
```

Configure `/etc/kibana/kibana.yml`:

```yaml
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]
```

### Install and Configure Filebeat

```bash
sudo apt install filebeat -y
sudo filebeat modules enable system
```

Configure `/etc/filebeat/modules.d/system.yml`:

```yaml
- module: system
  syslog:
    enabled: true
  auth:
    enabled: true
```

```bash
sudo filebeat setup --index-management -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'
sudo systemctl start filebeat
```

### Validation

```bash
curl http://localhost:9200
```

Expected output:
```json
{
  "name": "ubuntu-lab",
  "cluster_name": "elasticsearch",
  "version": { "number": "8.17.12" },
  "tagline": "You Know, for Search"
}
```

---

## Part 2 — SSH Brute Force Attack Simulation

### Attack Details

| Item | Detail |
|------|--------|
| Tool | Hydra 9.5 |
| Attacker | Kali Linux (172.20.10.7) |
| Target | Ubuntu SSH (172.20.10.2:22) |
| Username | root |
| Wordlist | rockyou.txt |

### Attack Command

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt 172.20.10.2 ssh -t 4 -V
```

### Verify Logs Being Generated

```bash
sudo tail -f /var/log/auth.log
```

Expected output:
```
Failed password for root from 172.20.10.7 port XXXXX ssh2
Failed password for root from 172.20.10.7 port XXXXX ssh2
```

---

## Part 3 and 4 — Detection and Analysis in Kibana

### Kibana Detection Queries

```
# All failed SSH attempts
Failed password

# Filter by attacker IP
Failed password for root from 172.20.10.7

# Filter by targeted account
Failed password for root
```

### Findings

| Item | Finding |
|------|---------|
| Attack Type | SSH Brute Force |
| Attacker IP | 172.20.10.7 (Kali) |
| Target IP | 172.20.10.2 (Ubuntu) |
| Target Account | root |
| Successful Logins | None |
| Detection Method | Kibana Discover + auth.log |

---

## MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|-----|-------------|
| Brute Force | T1110 | Automated password guessing |
| Password Guessing | T1110.001 | Rockyou wordlist cycling |
| Remote Services SSH | T1021.004 | SSH as attack vector |
| Tactic | Initial Access | Credential Access |

---

## NIST Incident Response

| Phase | Action |
|-------|--------|
| Preparation | SIEM deployed, auth.log monitored, Filebeat shipping logs |
| Detection | High volume failed SSH attempts detected in Kibana |
| Containment | Block attacker IP via ufw, disable root SSH login |
| Eradication | Review all auth logs, verify no successful logins |
| Recovery | Confirm normal authentication, monitor for repeat attempts |
| Post Incident | Implement fail2ban, enforce SSH key auth, set Kibana alerts |

---

## Recommendations

| Priority | Recommendation |
|----------|----------------|
| Critical | Disable root SSH login — set PermitRootLogin no |
| High | Implement fail2ban for automated brute force blocking |
| High | Enforce SSH key-based authentication only |
| High | Restrict SSH access to known IP ranges via firewall |
| Medium | Set up Kibana threshold alerts for failed auth attempts |
| Medium | Implement MFA for SSH access |
| Low | Move SSH to non-standard port to reduce scanning |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Elasticsearch 8.17.12 | Log indexing and storage |
| Kibana 8.17.12 | SIEM dashboard and analysis |
| Filebeat 8.17.12 | Log shipping from auth.log |
| Hydra 9.5 | SSH brute force simulation |
| Kali Linux | Attacker and analyst workstation |
| Ubuntu 24.04 | SIEM server and attack target |

---

## References

- [Elastic Documentation](https://www.elastic.co/docs)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [NIST Incident Response Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
- [Hydra Documentation](https://github.com/vanhauser-thc/thc-hydra)
