# 🔐 SOC Home Lab — Security Operations Center Simulation

> **Built by:** Vinit Limbachiya  
> **Role Target:** SOC Analyst  
> **Duration:** 5 Days  
> **Environment:** VMware Workstation Pro (Local) | Remote-Ready Documentation

---

## 📌 Project Overview

This project simulates a real-world **Security Operations Center (SOC)** environment built entirely on a local machine using VirtualBox. The lab covers the full SOC workflow — from setting up a monitored network, simulating cyber attacks, detecting threats via SIEM, and writing incident response reports.

**Key skills demonstrated:**
- SIEM deployment and log analysis (Splunk)
- Attack simulation using Kali Linux, Nmap, Metasploit, Hydra
- Threat detection, alerting, and correlation rules
- Incident Response (IR) documentation
- Network traffic monitoring and analysis

---

## 🏗️ Lab Architecture

```
Internet
    │
[pfSense Firewall] ── Network monitor, traffic filtering
    │
    ├── [Kali Linux VM]       → Attacker machine (192.168.136.129)
    ├── [Windows Server VM]    → Target/Victim machine (192.168.136.131)
    └── [Splunk VM]           → SIEM / Log collector (192.168.136.132)

Network: VMware Workstation Pro Host-Only Adapter — 192.168.136.0/24
```

---

## 🧰 Tools & Technologies

| Category | Tools Used |
|---|---|
| Virtualization | VMWare 7.x |
| Attacker OS | Kali Linux 2024 |
| Target OS | Windows Server 22.04 LTS |
| Firewall | pfSense 2.7 |
| SIEM | Splunk Free (Universal Forwarder) |
| Attack Tools | Nmap, Metasploit, Hydra |
| Monitoring | Splunk dashboards, pfSense logs |
| Documentation | Markdown, Incident Report Template |

---

## 📋 Phase Breakdown

### ✅ Phase 1 — Environment Setup (Day 1)
- Installed VirtualBox and configured 3 VMs on isolated Host-Only network
- Set up Kali Linux as attacker, Ubuntu Server as target, Splunk VM as SIEM
- Configured pfSense firewall for network segmentation and traffic logging
- Verified VM-to-VM connectivity using `ping` and `nmap`

**Screenshot:** `screenshots/phase1_network_setup.png`

---

### ✅ Phase 2 — Splunk SIEM Deployment (Day 2)
- Installed Splunk Free on dedicated VM
- Installed Universal Forwarder on Ubuntu target to ship logs to Splunk
- Created custom dashboards for: failed logins, port scans, SSH attempts
- Wrote SPL (Splunk Search Processing Language) queries for threat detection

**Sample SPL Query — Detect Port Scans:**
```spl
index=main sourcetype=syslog
| stats count by src_ip, dest_port
| where count > 20
| sort -count
```

**Screenshot:** `screenshots/phase2_splunk_dashboard.png`

---

### ✅ Phase 3 — Attack Simulation (Day 3)
Simulated the following attacks from Kali Linux against Ubuntu target:

| Attack Type | Tool Used | Purpose |
|---|---|---|
| Network reconnaissance | Nmap | Port and service discovery |
| Vulnerability scan | Nmap NSE scripts | CVE identification |
| Exploitation | Metasploit | Gaining shell access |
| Brute force SSH | Hydra | Password attack simulation |

**Screenshot:** `screenshots/phase3_attack_simulation.png`

---

### ✅ Phase 4 — Threat Detection & Incident Response (Day 4)
- Configured Splunk alerts triggered by attack signatures detected in logs
- Performed log correlation to map attack timeline
- Wrote a full **Incident Response Report** (see `/reports/IR_Report_001.md`)
- Followed NIST IR framework: Preparation → Detection → Containment → Eradication → Recovery

**IR Report Summary:**
```
Incident ID  : IR-2026-001
Date         : 8-06-2026
Severity     : High
Type         : Unauthorized port scan + brute force attempt
Source IP    : 192.168.136.129 (Kali VM)
Target IP    : 192.168.136.131 (Ubuntu VM)
Detection    : Splunk alert — SSH brute force rule triggered
Action Taken : IP blocked via pfSense, SSH hardened
```

**Screenshot:** `screenshots/phase4_splunk_alerts.png`

---

### ✅ Phase 5 — Documentation & Reporting (Day 5)
- Documented all findings in structured Incident Reports
- Created this README for GitHub publication
- Added project to resume under **Projects** section
- Linked GitHub repo in LinkedIn profile

---

## 📁 Repository Structure

```
SOC-HomeLab/
│
├── README.md                   ← This file
├── screenshots/
│   ├── phase1_network_setup.png
│   ├── phase2_splunk_dashboard.png
│   ├── phase3_attack_simulation.png
│   └── phase4_splunk_alerts.png
│
├── splunk-queries/
│   ├── port_scan_detection.spl
│   ├── brute_force_detection.spl
│   └── failed_login_alert.spl
│
├── reports/
│   └── IR_Report_001.md
│
└── configs/
    ├── splunk_forwarder_setup.md
    └── pfsense_rules.md
```

---

## 🔍 Key Findings & Learnings

1. **Nmap scans** generate a very distinctive pattern in Splunk logs — high port count from single source IP in short timeframe
2. **SSH brute force** is easily detectable via failed login count threshold in `/var/log/auth.log`
3. **pfSense** provides granular traffic visibility even for internal VM-to-VM traffic
4. **Splunk correlation rules** are powerful — chaining 2 alerts (scan + brute force from same IP) raises confidence of true positive

---

## 📄 Sample Incident Report

See full report: [📄View Full IR Report](./reports/IR_Report_001.md)

---

## 🚀 How to Replicate This Lab

1. Install VMWare (https://knowledge.broadcom.com/external/article/368667/download-and-license-vmware-desktop-hype.html)
2. Download Kali Linux OVA (kali.org/get-kali → Virtual Machines)
3. Download Ubuntu Server 22.04 ISO (ubuntu.com)
4. Download Splunk Free (splunk.com/en_us/download.html)
5. Set all VMs to Host-Only Adapter in VirtualBox Network settings
6. Follow phase-by-phase setup in `/configs/` folder

---

## 👤 About Me

**Vinit Limbachiya**  
Cybersecurity enthusiast | Security Researcher on HackerOne | SOC Analyst 

- 📧 vinitlimbachiya01@gmail.com  
- 🔗 [LinkedIn](https://linkedin.com/in/vinitlimbachiya)  
- 💻 [GitHub](https://github.com/vinitlimbachiya)  
- 🐛 HackerOne — Active security researcher (XSS, SQLi, IDOR, CSRF)

---

## 📜 Certifications Relevant to This Project

- Splunk Core Certified User
- Security Blue Team — Intro to Penetration Testing
- Tech Mahindra — Cyber Security
- IBM — Python 101 for Data Science

---

*This lab was built as a hands-on project to demonstrate real SOC analyst skills including threat monitoring, SIEM management, attack simulation, and incident response.*
