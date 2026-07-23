# 🔍 Project 2 — SIEM Deployment & Log Analysis with Wazuh

![Security](https://img.shields.io/badge/Type-SIEM%20Lab-blue)
![Tools](https://img.shields.io/badge/Tools-Wazuh%20%7C%20Ubuntu%20Server%20%7C%20Windows%2010-darkgreen)
![Level](https://img.shields.io/badge/Level-Intermediate-orange)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

## 📋 Overview

Deployed a full Wazuh SIEM stack on Ubuntu Server 22.04,
connected a Windows 10 endpoint as a live agent, simulated
real-world attacks, and captured MITRE ATT&CK mapped alerts
in the dashboard. Wrote custom detection rules for port
scanning and brute force attacks.

This project demonstrates:
- SIEM deployment and configuration
- Endpoint agent management
- Attack simulation and detection
- Custom detection rule writing
- MITRE ATT&CK framework mapping
- Alert triage and investigation

---

## 🏗️ Lab Architecture

Internet (VirtualBox NAT)
│
▼
┌─────────────────────────┐
│ pfSense Firewall VM │
│ 192.168.1.1 │
└─────────┬───────────────┘
│ LabNet (192.168.1.0/24)
│
┌─────┴──────────────┐
▼ ▼
┌──────────────┐ ┌─────────────┐
│ Ubuntu Server│ │ Windows 10 │
│ 192.168.1.50 │ │ 192.168.1.101│
│ Wazuh Manager│ │ Wazuh Agent │
│ + Dashboard │ │ Windows10-Lab│
└──────────────┘ └─────────────┘


---

## 🛠️ Tools Used

| Tool | Version | Purpose |
|---|---|---|
| Wazuh Manager | 4.7.5 | SIEM engine — receives and analyses logs |
| Wazuh Dashboard | 4.7.5 | Web UI — visualizes alerts and events |
| Wazuh Indexer | 4.7.5 | OpenSearch — stores all log data |
| Wazuh Agent | 4.7.5 | Windows 10 endpoint log collection |
| Ubuntu Server | 22.04.5 LTS | Wazuh Manager host |
| Windows 10 Pro | 10.0.19041 | Monitored endpoint |
| pfSense | 2.6.0 | Network firewall/gateway |
| VirtualBox | 7.x | Hypervisor |

---

## ⚙️ Wazuh Components Explained

| Component | Role | Port |
|---|---|---|
| Wazuh Manager | Receives logs, runs rules, fires alerts | 1514/1515 |
| Wazuh Indexer | Stores and indexes all log data | 9200 |
| Wazuh Dashboard | Web interface for analysts | 443 |
| Wazuh Agent | Installed on endpoints, sends logs | 1514 |

---

## 🔍 Attack Simulations Run

### Attack 1: SSH Brute Force
**Target:** Ubuntu Server (192.168.1.50)
**Method:** Repeated SSH login attempts with
wrong credentials from Ubuntu Desktop

**Wazuh Detection:**
| Rule ID | Level | Description | MITRE |
|---|---|---|---|
| 5710 | 5 | SSH login attempt with non-existent user | T1110.001 |
| 5503 | 5 | PAM authentication failure | T1110.001 |
| 2502 | 10 | Multiple password failures | T1110 |

**Tactics detected:** Credential Access, Lateral Movement

---

### Attack 2: Windows Brute Force
**Target:** Windows 10 VM (192.168.1.101)
**Method:** Multiple failed login attempts
on Windows lock screen

**Wazuh Detection:**
| Rule ID | Level | Description | MITRE |
|---|---|---|---|
| 60122 | 5 | Windows authentication failure | T1110 |
| 60106 | 3 | Windows logon success | T1078 |
| 60137 | 3 | Windows user logoff | T1078 |

**Tactics detected:** Credential Access,
Defense Evasion, Persistence

---

### Attack 3: Nmap Port Scan
**Target:** Windows 10 VM (192.168.1.101)
**Method:** Nmap service version scan
from Ubuntu Desktop

**Command used:**
```bash
sudo nmap -sV -p 1-1000 192.168.1.101
```

---

## 📝 Custom Detection Rules Written

See [rules/custom_detection_rules.md](./rules/custom_detection_rules.md)
and [configs/local_rules.xml](./configs/local_rules.xml)

| Rule ID | Level | Trigger | MITRE | Tactic |
|---|---|---|---|---|
| 100001 | 10 | Port scan detected | T1046 | Discovery |
| 100002 | 12 | Multiple auth failures (3 in 2 min) | T1110 | Credential Access |
| 100003 | 12 | Multiple SSH failures same IP | T1110 | Credential Access |

---

## 🎯 MITRE ATT&CK Coverage

| Technique | ID | Tactic | Detected By |
|---|---|---|---|
| Brute Force | T1110 | Credential Access | Rules 2502, 100002 |
| Valid Accounts | T1078 | Defense Evasion | Rule 60106 |
| SSH | T1021.004 | Lateral Movement | Rule 5710 |
| Network Service Discovery | T1046 | Discovery | Rule 100001 |
| Brute Force: Password Guessing | T1110.001 | Credential Access | Rule 5710 |

---

## 📸 Evidence

| Evidence | Description |
|---|---|
| [Dashboard Overview](./screenshots/wazuh_dashboard_overview.png) | Main dashboard showing alert statistics |
| [Agents Page](./screenshots/wazuh_agents_active.png) | Windows10-Lab agent active |
| [SSH Brute Force Alerts](./screenshots/ssh_brute_force_detection.png) | Real SSH attack detection |
| [Alert Detail](./screenshots/alert_detail_rule5710.png) | Expanded alert with full details |
| [Windows Alerts](./screenshots/windows_brute_force_alerts.png) | Windows brute force detection |

---

## 📚 Key Lessons Learned

See full documentation: [lessons_learned.md](./lessons_learned.md)

**Highlights:**
- Wazuh service startup order is critical
- Detection rule specificity vs coverage tradeoff
- if_matched_sid vs if_matched_group difference
- SSH is the most attacked Linux service
- MITRE ATT&CK mapping adds context to raw alerts
- VM resource allocation affects SIEM performance

---

## 🎯 Skills Demonstrated
SIEM Deployment ████████████████░░░░ Intermediate
Log Analysis ████████████████░░░░ Intermediate
Detection Engineering ████████████░░░░░░░░ Foundational
MITRE ATT&CK ████████████████░░░░ Intermediate
Linux Administration ████████████████░░░░ Intermediate
Attack Simulation ████████████░░░░░░░░ Foundational
Incident Triage ████████████░░░░░░░░ Foundational


---

## 🔜 What's Next

This is **Project 2 of 11** in my cybersecurity portfolio.

**Project 3:** Python Security Automation Toolkit
- Log parser and IP reputation checker
- Port scanner and password auditor
- AbuseIPDB API integration

**Full roadmap:**
[github.com/kamalanathankaviprasath-cyber](https://github.com/kamalanathankaviprasath-cyber)

---

*Built by Kamalanathan Kaviprasath*
*IT Support Professional → Cybersecurity Engineer*
*📍 Colombo, Sri Lanka*
