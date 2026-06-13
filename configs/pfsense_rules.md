[pfsense_rules.md](https://github.com/user-attachments/files/28910458/pfsense_rules.md)

# pfSense Firewall Rules — SOC Home Lab

## Environment
- **pfSense Version:** 2.7.x CE
- **WAN Interface:** NAT (Internet access)
- **LAN Interface:** VMnet8 (192.168.136.0/24)

---

## Lab Network Layout

```
Internet
    │
[pfSense]
    │
VMnet8 (192.168.136.0/24)
    ├── Kali Linux      192.168.136.129  (Attacker)
    ├── Windows Server  192.168.131.136  (Victim)
    └── Splunk SIEM     192.168.136.132  (Monitor)
```

---

## Inbound Rules (LAN)

| Rule | Protocol | Source | Destination | Port | Action |
|---|---|---|---|---|---|
| Allow Splunk UI | TCP | Any | 192.168.136.132 | 8000 | Allow |
| Allow Splunk Forwarder | TCP | Any | 192.168.136.132 | 9997 | Allow |
| Allow RDP | TCP | Kali (136.129) | WinServer (131.136) | 3389 | Allow |
| Allow SMB | TCP | Kali (136.129) | WinServer (131.136) | 445 | Allow |
| Allow ICMP | ICMP | Any | Any | — | Allow |
| Block All Others | Any | Any | Any | Any | Block |

---

## Post-Incident Rules (Containment)

After Metasploit attack detected — these rules were added:

| Rule | Protocol | Source | Destination | Port | Action |
|---|---|---|---|---|---|
| Block SMB from Kali | TCP | 192.168.136.129 | 192.168.131.136 | 445 | Block |
| Block RDP from Kali | TCP | 192.168.136.129 | 192.168.131.136 | 3389 | Block |

---

## Adding Rules in pfSense UI

```
pfSense → Firewall → Rules → LAN
→ Add Rule
→ Protocol: TCP
→ Source: Single Host → <IP>
→ Destination: Single Host → <IP>
→ Port: <port>
→ Action: Pass / Block
→ Save → Apply Changes
```

---

## Key Observations

- SMB (445) should be **disabled** in production if not required
- pfSense logs showed all attack traffic in real-time
- Firewall rules effectively contained the Metasploit session post-detection
