[splunk_forwarder_setup.md](https://github.com/user-attachments/files/28910451/splunk_forwarder_setup.md)

# Splunk Universal Forwarder Setup — Windows Server

## Environment
- **Forwarder OS:** Windows Server 2012 R2
- **Splunk SIEM IP:** 192.168.136.132
- **Receiving Port:** 9997

---

## Step 1 — Download
Download Splunk Universal Forwarder 9.x (Windows 64-bit .msi):
```
https://splunk.com/en_us/download/universal-forwarder.html
```

## Step 2 — Install
Run `.msi` as Administrator with these settings:
```
Username         : admin
Password         : <your-password>
Deployment Server: <blank>
Receiving Indexer: 192.168.136.132:9997
```

## Step 3 — Configure Receiving Port on Splunk SIEM
```
Splunk UI → Settings → Forwarding and Receiving
→ Configure Receiving → New Receiving Port → 9997
```

## Step 4 — Add Windows Event Log Monitors
Run CMD as Administrator on Windows Server:
```cmd
cd "C:\Program Files\SplunkUniversalForwarder\bin"

splunk add monitor "C:\Windows\System32\winevt\Logs\Security.evtx" -index main -sourcetype WinEventLog:Security -auth admin:<password>

splunk add monitor "C:\Windows\System32\winevt\Logs\System.evtx" -index main -sourcetype WinEventLog:System -auth admin:<password>

splunk add monitor "C:\Windows\System32\winevt\Logs\Application.evtx" -index main -sourcetype WinEventLog:Application -auth admin:<password>
```

## Step 5 — Restart Forwarder
```cmd
splunk restart
```

## Step 6 — Verify in Splunk
```spl
index=main sourcetype="WinEventLog:Security"
```
Should return events from WIN-6V0Q0OU4TDO ✅

---

## Troubleshooting

| Issue | Fix |
|---|---|
| No logs in Splunk | Check port 9997 open in firewall |
| Forwarder not running | Run `splunk status` in CMD |
| Disk space error | Free up space — Splunk needs 5GB minimum |
