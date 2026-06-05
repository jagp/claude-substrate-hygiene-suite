# procedure.md — Run Algorithm
> Canonical autonomous run procedure.
> Last updated: 2026-06-05

---

## Scan Modes

Two modes. Choose based on cadence and intent.

| Mode | Frequency | Scope |
|------|-----------|-------|
| **Shallow** | Weekly | Defender quick scan, disk check, errors.md review, FRST scan (analysis only if flagged) |
| **Deep** | Monthly | Full app chain — all phases below, in sequence |

Shallow scans are designed to be low-token: scripts run autonomously, I'm invoked only if something is flagged. Most weeks, nothing happens. Schedule stub in `ROADMAP.md`.

---

## Phase 0 — Pre-Session Checks

**Trigger:** Any scan mode, or after major installs, suspected infection, performance issues, Windows Update failures.

1. Verify C: free space > 15 GB. If below, halt and log to `errors.md` — disk cleanup is prerequisite.
2. Verify Defender real-time protection is active:
   ```powershell
   Get-MpPreference | Select DisableRealtimeMonitoring
   ```
3. Verify system is not in Safe Mode.
4. Confirm internet connectivity (required for HitmanPro cloud lookups).
5. Create restore point:
   ```powershell
   Checkpoint-Computer -Description "Pre-Suite-Run" -RestorePointType MODIFY_SETTINGS
   ```

---

## Phase 1 — FRST Scan

1. Download latest `FRST64.exe` to `%USERPROFILE%\Downloads\` (always fresh — auto-updates).
2. Run as Administrator.
3. Click **Scan** — do NOT click Fix yet.
4. FRST generates: `FRST.txt`, `Addition.txt`, `Shortcut.txt`.
5. Archive all three to `./FRST/archive/[YYYY-MM-DD]/`.
6. Upload all three for analysis.

---

## Phase 2 — FRST Analysis and FIXLIST Generation

### 2a. Auto-include in FIXLIST (safe)
- Startup registry entries where target is `(No File)`
- Scheduled tasks where executable is `(No File)`
- Shell overlay identifiers pointing to non-existent DLLs
- Custom CLSID entries for removed software `(No File)`
- Orphaned context menu handlers `(No File)`
- Firewall rules where target path is `=> No File`
- Stale `.job` task files for removed programs

### 2b. Flag for manual review — do NOT auto-fix
- Items marked `<==== ATTENTION` by FRST
- Services or drivers with no ImagePath
- Security Center entries showing conflicting AV engines
- Accounts of unknown origin
- Group Policy restrictions of unknown origin
- Any item where removal could affect active functionality
- Suspected malware requiring human verification

### 2c. FIXLIST Generation Rules
- FIXLIST contains **only** items from 2a
- Each entry must be the exact verbatim line from FRST output
- Save to `./FRST/fixlists/[YYYY-MM-DD]-FIXLIST.txt`
- Flagged items documented in `./logs/errors.md` with severity and required action
- Activity log entry created in `./logs/[YYYY-MM-DD]-activity.md`

---

## Phase 3 — FIXLIST Deployment

1. Review `./FRST/fixlists/[YYYY-MM-DD]-FIXLIST.txt`.
2. Copy to `%USERPROFILE%\Downloads\FIXLIST.txt`.
3. Run FRST64.exe as Administrator.
4. Click **Fix**.
5. Archive `Fixlog.txt` alongside FIXLIST.
6. Reboot if prompted.
7. Run Phase 1 again to confirm fixes applied.

---

## Phase 4 — Secondary Scanner Sweep

Run in order after FRST fix pass:

1. **HitmanPro** — quick scan, review detections, apply with restore point
2. **AdwCleaner** — scan → review → clean (reboot if required)
3. **MalwareBytes** — threat scan, quarantine detections
4. **Windows Defender** — `Start-MpScan -ScanType FullScan` (quick scan for shallow mode)
5. **360 TS** — on-demand scan (supplemental)

App chain orchestration lives in `./apps/`. See `ROADMAP.md` for parallel execution plans.

---

## Phase 5 — Defender Health Check

```powershell
Get-MpPreference | Select DisableRealtimeMonitoring, DisableIOAVProtection
Get-MpThreatDetection | Select-Object -Last 10
Get-MpComputerStatus | Select AntivirusSignatureLastUpdated, NISSignatureLastUpdated
```

If real-time protection is disabled or definitions are >24h old, investigate before proceeding.

---

## Phase 6 — Logging and Wrap-Up

1. Append summary to `./logs/[YYYY-MM-DD]-activity.md`.
2. Update `./logs/errors.md` — add new flags, mark resolved items.
3. Update `./logs/config.md` if tools or configuration changed.
4. Check disk: if C: < 15 GB free, add cleanup task to `errors.md`.

---

## Escalation Criteria

Halt autonomous run and flag immediately:
- `<==== ATTENTION` FRST finding that is NOT a simple No-File orphan
- Suspected active malware (running process/service with unknown signature)
- AV engine conflicts (multiple real-time AV simultaneously)
- Firewall disabled unexpectedly
- New account not present in last scan
- Disk space < 5 GB
- Windows Update failing with non-trivial error codes
- BSOD / critical stop in event log
