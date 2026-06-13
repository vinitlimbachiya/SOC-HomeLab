[IR_Report_001.md](https://github.com/user-attachments/files/28910363/IR_Report_001.md)

# Incident Response Report — IR-2026-001

| Field | Details |
|---|---|
| **Incident ID** | IR-2026-001 |
| **Date** | June 08, 2026 |
| **Prepared By** | Vinit Limbachiya — SOC Analyst (Trainee) |
| **Severity** | HIGH |
| **Status** | RESOLVED |
| **Classification** | CONFIDENTIAL — Internal Use Only |

---

## 1. Executive Summary

Three simulated cyber attacks were conducted against a controlled Windows Server 2012 R2 environment as part of a SOC Home Lab exercise. All attacks were initiated from a Kali Linux attacker machine and detected using Splunk Enterprise SIEM.

| Attack | Tool | Detection | Status |
|---|---|---|---|
| Network Reconnaissance | Nmap | Splunk Network Logs | Detected ✅ |
| SMB Brute Force | Hydra | EventCode 4625 | Detected ✅ |
| SMB Exploitation | Metasploit Psexec | EventCode 4624 (Type 3) | Detected ✅ |

---

## 2. Lab Environment

| VM | OS | Role | IP Address |
|---|---|---|---|
| Kali Linux | Kali Linux 2024 | Attacker | 192.168.136.129 |
| Windows Server | Windows Server 2012 R2 | Target / Victim | 192.168.131.136 |
| Ubuntu (Splunk) | Ubuntu Server 22.04 | SIEM | 192.168.136.132 |

**Network:** VMware Workstation Pro — VMnet8 (NAT)

---

## 3. Incident Timeline

| Time | Phase | Activity |
|---|---|---|
| 06:00 AM | Reconnaissance | Nmap full port scan from 192.168.136.129 → 192.168.131.136 |
| 07:10 AM | Credential Attack | Hydra SMB brute force — 10 password attempts on port 445 |
| 07:19 AM | Detection | Splunk detected 10x EventCode 4625 — Brute Force alert triggered |
| 07:44 AM | Exploitation | Metasploit psexec successful — Meterpreter session opened |
| 07:45 AM | Detection | Splunk detected EventCode 4624 Logon_Type=3 — Suspicious Login alert |
| 08:00 AM | Containment | Session terminated, Firewall rules updated |

---

## 4. Attack Details

### 4.1 Nmap Reconnaissance
- **Command:** `nmap -sV -p- 192.168.131.136`
- **Open Port:** 5985/tcp — Microsoft HTTPAPI (WinRM)
- **MITRE ATT&CK:** T1046 — Network Service Discovery

### 4.2 Hydra SMB Brute Force
- **Command:** `hydra -l Administrator -P passlist.txt smb://192.168.131.136 -v -I`
- **Attempts:** 10 password attempts on port 445
- **Result:** 0 valid passwords (password not in wordlist)
- **Splunk Detection:** 10x EventCode 4625
- **MITRE ATT&CK:** T1110.001 — Brute Force: Password Guessing

### 4.3 Metasploit Psexec Exploitation
- **Module:** `exploit/windows/smb/psexec`
- **Payload:** `windows/meterpreter/reverse_tcp`
- **Result:** Meterpreter Session 1 opened — SYSTEM access
- **Splunk Detection:** EventCode 4624, Logon_Type=3
- **MITRE ATT&CK:** T1021.002 — Remote Services: SMB/Windows Admin Shares

---

## 5. Splunk Detection

| EventCode | Event | Count |
|---|---|---|
| 4625 | Failed Login (Brute Force) | 10 |
| 4624 | Successful Login | 78 |
| 4672 | Admin Privileges Login | 70 |

### Alerts Created
| Alert Name | Trigger |
|---|---|
| Brute Force Detection | EventCode 4625 count > 5 |
| Suspicious Login Pattern | EventCode 4624 Logon_Type=3 > 0 |

---

## 6. NIST Incident Response Framework

| Phase | Actions |
|---|---|
| **Preparation** | Splunk SIEM configured, Universal Forwarder deployed, alerting rules set |
| **Detection** | EventCode 4625 (brute force) and 4624 Logon_Type=3 (exploitation) detected |
| **Analysis** | Attack origin confirmed: 192.168.136.129, MITRE ATT&CK techniques mapped |
| **Containment** | Meterpreter session terminated, SMB port 445 restricted |
| **Eradication** | Payload removed, firewall hardened, strong password enforced |
| **Recovery** | System restored, Splunk monitoring confirmed clean |
| **Lessons Learned** | SMB should be disabled if unused, MFA recommended for Administrator |

---

## 7. Recommendations

| # | Recommendation | Priority |
|---|---|---|
| 1 | Disable SMB (Port 445) if not required | CRITICAL |
| 2 | Implement Account Lockout after 5 failed attempts | HIGH |
| 3 | Enable MFA for Administrator account | HIGH |
| 4 | Rename default Administrator account | MEDIUM |
| 5 | Apply Windows patches regularly | HIGH |
| 6 | Increase Splunk alert frequency to real-time | MEDIUM |

---

## 8. Sign-Off

**Prepared by:** Vinit Limbachiya  
**Email:** vinitlimbachiya01@gmail.com  
**GitHub:** [github.com/vinitlimbachiya/SOC-HomeLab](https://github.com/vinitlimbachiya/SOC-HomeLab)  
**LinkedIn:** [linkedin.com/in/vinitlimbachiya](https://linkedin.com/in/vinitlimbachiya)  
**Certification:** Splunk Core Certified User  

*— End of Report —*
