# procedure.md — Run Algorithm
> Canonical run procedure. Loaded on demand — see `CLAUDE.md` routing table.
> Last updated: 2026-06-05

---

## Scan modes

| Mode | Cadence | Scope |
|------|---------|-------|
| **shallow** | Weekly | Defender quick scan, disk check, errors.md review, FRST scan (analyze only if flagged) |
| **deep** | Monthly | Full toolchain — all phases below in sequence |

Shallow is low-token by design: scripts run autonomously, analysis invoked only on flags. Schedule stub in `ROADMAP.md`.

---

## Phase 0 — pre-session checks

Trigger: any scan mode, or after major installs, suspected infection, performance issues, update failures.

1. Verify C: free space > 15 GB → halt and log to `errors.md` if below
2. Verify Defender real-time protection active:
   ```powershell
   Get-MpPreference | Select DisableRealtimeMonitoring
   ```
3. Verify not in Safe Mode
4. Confirm internet (required for HitmanPro cloud lookups)
5. Create restore point:
   ```powershell
   Checkpoint-Computer -Description "pre-suite-run" -RestorePointType MODIFY_SETTINGS
   ```

---

## Phase 1 — FRST scan

1. Download latest `FRST64.exe` to `%USERPROFILE%\Downloads\` (always fresh — self-updates)
2. Run as Administrator
3. Click **Scan** — do NOT click Fix yet
4. Archive `FRST.txt`, `Addition.txt`, `Shortcut.txt` to `./apps/frst/archive/[yyyy-mm-dd]/`
5. Upload all three for analysis

---

## Phase 2 — FRST analysis and FIXLIST generation

### 2a. Auto-include in FIXLIST (safe)
- Startup entries where target is `(No File)`
- Scheduled tasks where executable is `(No File)`
- Shell overlay identifiers pointing to missing DLLs
- CLSID entries for removed software `(No File)`
- Orphaned context menu handlers `(No File)`
- Firewall rules where target path is `=> No File`
- Stale `.job` files for removed programs

### 2b. Flag for manual review — do NOT auto-fix
- Items marked `<==== ATTENTION`
- Services or drivers with no ImagePath
- Security Center entries with conflicting AV engines
- Accounts of unknown origin
- Group Policy restrictions of unknown origin
- Anything where removal could affect active functionality
- Suspected malware requiring human verification

### 2c. FIXLIST rules
- Contains **only** items from §2a
- Each entry verbatim from FRST output
- Save to `./apps/frst/fixlists/[yyyy-mm-dd]-FIXLIST.txt`
- Flagged items → `./logs/errors.md` with severity and required action
- Activity entry → `./logs/[yyyy-mm-dd]-activity.md`

---

## Phase 3 — FIXLIST deployment

1. Review `./apps/frst/fixlists/[yyyy-mm-dd]-FIXLIST.txt`
2. Copy to `%USERPROFILE%\Downloads\FIXLIST.txt`
3. Run FRST64.exe as Administrator → click **Fix**
4. Archive `Fixlog.txt` alongside FIXLIST
5. Reboot if prompted
6. Re-run Phase 1 to confirm fixes applied

---

## Phase 4 — secondary scanner sweep

Run in order after FRST fix pass:

1. **HitmanPro** → quick scan, review, apply with restore point → archive to `./apps/hitmanpro/archive/`
2. **AdwCleaner** → scan → review → clean (reboot if required) → archive to `./apps/adwcleaner/archive/`
3. **MalwareBytes** → threat scan, quarantine → archive to `./apps/malwarebytes/archive/`
4. **Defender** → `Start-MpScan -ScanType FullScan` (QuickScan for shallow mode) → archive to `./apps/windefender/archive/`
5. **360 TS** → on-demand scan (supplemental)

---

## Phase 5 — Defender health check

```powershell
Get-MpPreference | Select DisableRealtimeMonitoring, DisableIOAVProtection
Get-MpThreatDetection | Select-Object -Last 10
Get-MpComputerStatus | Select AntivirusSignatureLastUpdated, NISSignatureLastUpdated
```

Investigate if real-time protection is disabled or definitions are >24h old.

---

## Phase 6 — logging and wrap-up

1. Append summary to `./logs/[yyyy-mm-dd]-activity.md`
2. Update `./logs/errors.md` — add new flags, mark resolved
3. Update `./logs/config.md` if tools or config changed
4. Check disk: C: < 15 GB free → add cleanup task to `errors.md`

---

## Escalation criteria

Halt and flag immediately — see `CLAUDE.md` escalation list.
