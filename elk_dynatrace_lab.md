# 🚀 Cybersecurity Lab: ELK Stack + Filebeat + Dynatrace + Ansible

Beginner-friendly cybersecurity monitoring lab using **free and open-source tools**. Built using Docker, VirtualBox, and Linux terminal — ideal for anyone looking to demonstrate real-world skills on GitHub or LinkedIn.

---

## 📦 Tools Used

- **Ubuntu Server** (VM via VirtualBox)
- **Docker & Docker Compose**
- **Elastic Stack (ELK)**: Elasticsearch, Logstash, Kibana
- **Filebeat** (for shipping logs)
- **Dynatrace OneAgent** (optional)
- **Ansible** (optional for automation)

---

## 🛠️ Step-by-Step Lab Setup

### 1. Install Docker & Docker Compose

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker && sudo systemctl start docker
```

---

### 2. Clone and Launch ELK Stack

```bash
git clone https://github.com/deviantony/docker-elk.git
cd docker-elk
docker-compose up -d
```

📌 Access Kibana via your VM IP (e.g. [http://192.168.56.101:5601](http://192.168.56.101:5601))

```bash
ip a  # to find IP address
```

---

### 3. Install and Configure Filebeat on Host

```bash
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.17.9-amd64.deb
sudo dpkg -i filebeat-7.17.9-amd64.deb
```

Edit `/etc/filebeat/filebeat.yml` to forward logs to Logstash:

```yaml
output.logstash:
  hosts: ["localhost:5044"]
```

Enable modules and start Filebeat:

```bash
sudo filebeat modules enable system
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

---

### 4. Confirm Logs Are Reaching ELK

```bash
curl -XGET http://localhost:9200/_cat/indices?v
```

Expected output:

- Index named like `logstash-2025.07.27-000001` shows success

---

### 5. Configure Kibana Index Pattern

1. Go to **Management → Stack Management → Index Patterns**
2. Click **Create index pattern**
3. Enter `logstash-*`
4. Select `@timestamp` as the timestamp field
5. Go to **Discover** to view logs

---

## 🧪 Simulate Security Events

Install common tools:

```bash
sudo apt install auditd nmap
```

Example simulation:

```bash
sudo nmap localhost
```

Then, return to Kibana to observe event logs.

---

## 🧠 Troubleshooting

| Problem                                  | Solution                                                                   |
| ---------------------------------------- | -------------------------------------------------------------------------- |
| Filebeat logs empty                      | Run `sudo journalctl -u filebeat`                                          |
| `filebeat setup` error                   | Use Logstash output, not Elasticsearch                                     |
| Can't access Kibana from Ubuntu terminal | Use browser and VM IP ([http://192.168.X.X:5601](http://192.168.X.X:5601)) |
| No `@timestamp` in Kibana                | Ensure Logstash sets timestamp correctly                                   |

---

## 🧰 Optional Add-ons

- **Dynatrace**: Add OneAgent (free trial) or simulate metrics
- **Ansible**: Use playbooks to automate Filebeat or Docker installation

---

## 📚 Free Learning Resources

- [Elastic Docs – Filebeat → Logstash](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-logstash.html)
- [Docker-ELK GitHub](https://github.com/deviantony/docker-elk)
- [Try Dynatrace Free](https://www.dynatrace.com/trial/)

---

## 📢 Project Sharing Template

This project showcases the ability to:

- Deploy and manage the Elastic Stack
- Collect and visualize system logs
- Simulate attack data for monitoring
- Use Docker and Linux for security observability

Perfect for a GitHub repo or LinkedIn post to demonstrate hands-on experience in SIEM & system monitoring.

Feel free to fork, reuse, and contribute!

