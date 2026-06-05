# tools.md — Software Inventory
> Last updated: 2026-06-05

---

## Primary Security Suite

### FRST64 (Farbar Recovery Scan Tool)
- **Role:** System forensic scanner and repair engine
- **Run:** Administrator — `%USERPROFILE%\Downloads\FRST64.exe`
- **Scan output:** `FRST.txt`, `Addition.txt`, `Shortcut.txt` → archive to `./FRST/archive/[date]/`
- **Fix run:** Requires `FIXLIST.txt` in same directory as exe; click "Fix"
- **Update:** Re-download before each session (self-updates on download)
- **Produces:** `Fixlog.txt` after fix run — archive alongside FIXLIST

### HitmanPro 3.8
- **Role:** Cloud-based behavioral malware scanner (second opinion)
- **Run:** Administrator — `C:\Program Files\HitmanPro\HitmanPro_x64.exe`
- **Schedule:** Background scheduler active (`hmpsched.exe` service)
- **Quarantine:** In-app; creates restore point before fix
- **Update:** Auto-updates on launch

### AdwCleaner (Malwarebytes)
- **Role:** Adware, PUP, browser hijacker removal
- **Run:** Administrator — `%USERPROFILE%\Downloads\AdwCleaner.exe`
- **Workflow:** Scan → Review → Clean; reboot required after clean
- **Update:** Re-download to get latest version (portable)

### MalwareBytes
- **Role:** Primary on-demand + real-time malware scanner
- **Run:** Start Menu or system tray
- **Schedule:** Weekly quick scan; monthly full scan

### 360 Total Security
- **Role:** Supplemental AV + vulnerability scanner
- **Run:** System tray / Start Menu
- **Note:** Use for on-demand scanning; disable real-time if it conflicts with Defender

### Windows Defender / Microsoft Security
- **Role:** Primary real-time AV and firewall (OS-native)
- **Management:** PowerShell `Set-MpPreference`, `Start-MpScan`, `MpCmdRun.exe`
- **Firewall:** `netsh advfirewall` or Windows Defender Firewall app

---

## Supporting Tools

### PowerShell (elevated)
Key cmdlets:
```powershell
Start-MpScan -ScanType QuickScan
Set-MpPreference -DisableRealtimeMonitoring $false
Get-MpThreatDetection
Get-MpComputerStatus | Select AntivirusSignatureLastUpdated
```

### Windows Event Viewer — `eventvwr.msc`
### Task Scheduler — `taskschd.msc`
### Registry Editor — `regedit.exe` (Administrator; export affected keys before editing)
### Disk Cleanup — `cleanmgr /sageset:1` then `cleanmgr /sagerun:1`

---

## Planned / Under Evaluation

| Tool | Role |
|------|------|
| Autoruns (Sysinternals) | Deep autostart inventory; complements FRST |
| Process Monitor | Real-time process/registry/network activity |
| WinDirStat / TreeSize | Disk space visualization |
| Recuva | File recovery if needed after aggressive cleanup |
| O&O ShutUp10++ | Privacy and telemetry hardening |
