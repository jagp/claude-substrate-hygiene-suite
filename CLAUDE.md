# CLAUDE.md
> Session anchor — auto-loaded when this folder is mounted.
> See [`conventions.md`](conventions.md) for documentation standards.
> Last updated: 2026-06-05

---

## Purpose

Autonomous PC security and maintenance. I coordinate a suite of third-party scanning tools, analyze output, generate remediation artifacts, and maintain structured logs. Human review gates all fix deployment.

---

## File Map

```
./
├── CLAUDE.md              ← this file
├── README.md              ← project overview (public-facing)
├── procedure.md           ← run algorithm (scan modes, phases)
├── tools.md               ← software inventory
├── conventions.md         ← documentation standards
├── ROADMAP.md             ← deferred and planned work
├── .env.example           ← template for machine-specific values
├── .githooks/             ← pre-commit hooks
├── apps/                  ← app chain orchestration (stub)
├── FRST/
│   ├── archive/[date]/    ← scan output (gitignored)
│   ├── fixlists/          ← generated FIXLISTs (gitignored)
│   └── pending/           ← staged for review (gitignored)
├── HitmanPro/
│   └── archive/[date]/    ← scan output (gitignored)
├── AdwCleaner/
│   └── archive/[date]/    ← scan output (gitignored)
├── MalwareBytes/
│   └── archive/[date]/    ← scan output (gitignored)
├── WinDefender/
│   └── archive/[date]/    ← scan output (gitignored)
└── logs/
    ├── schema.md          ← log structure spec
    ├── errors.md          ← open flags (gitignored)
    ├── config.md          ← tool/config state (gitignored)
    └── [YYYY-MM-DD]-activity.md  ← run archive (gitignored)
```

---

## Session Start Checklist

1. Read `./logs/errors.md` — surface OPEN items immediately.
2. Read `./logs/config.md` — verify tool inventory.
3. If scan output files are provided, proceed to Phase 2 per `procedure.md`.
4. If no task given, report open errors and offer Phase 0 pre-checks.

---

## Autonomous vs. Manual

| Action | Who |
|--------|-----|
| Scan output analysis | Me |
| FIXLIST generation | Me |
| FIXLIST deployment | **Human** — always |
| Secondary scanner runs | Human-initiated; I analyze output |
| Registry edits | I draft; **Human** approves |
| Log maintenance | Me |
| Escalation decisions | I flag; **Human** decides |

---

## Log Conventions

See `./logs/schema.md` for full structure spec.

**errors.md** — Append new items; prune resolved. No versioning.
Entry format: `[DATE] [SEVERITY] — Description — Required Action`
Severity: `CRITICAL` · `HIGH` · `MEDIUM` · `INFO`

**[YYYY-MM-DD]-activity.md** — One per day. Concise stubs, scannable in <30s.
Sections: `## Summary` · `## Actions Taken` · `## Flags Raised` · `## Next Steps`

**config.md** — Update when tools change or config deviates from baseline.

---

## Escalation — Halt and Flag

Stop and surface immediately if:
- `<==== ATTENTION` FRST finding that is NOT a simple No-File orphan
- Suspected active malware (running process/service with unknown signature)
- Multiple real-time AV engines in conflict
- Windows Firewall unexpectedly disabled
- New account not present in last scan
- C: free space < 5 GB
- Windows Update failing with non-trivial error codes
- BSOD / critical stop in event log

---

## FIXLIST Rules

**Auto-include:** `(No File)` startup entries, scheduled tasks, shell overlays, CLSIDs, context menu handlers, firewall rules, stale `.job` files.

**Never include:** `<==== ATTENTION` items, services missing ImagePath, conflicting AV entries, unknown accounts, Group Policy entries, anything touching active functionality.

Full detail: `procedure.md §2`.

---

## Key Paths

Defaults use `%USERPROFILE%`. Machine-specific overrides in `.env`.

| Resource | Default |
|----------|---------|
| FRST64 | `%USERPROFILE%\Downloads\FRST64.exe` |
| FIXLIST deploy | `%USERPROFILE%\Downloads\FIXLIST.txt` |
| HitmanPro | `C:\Program Files\HitmanPro\HitmanPro_x64.exe` |
| AdwCleaner | `%USERPROFILE%\Downloads\AdwCleaner.exe` |
| MalwareBytes | Start Menu / system tray |
| Defender | PowerShell (elevated): `Get-MpPreference`, `Start-MpScan` |

---

## References

`procedure.md` · `tools.md` · `conventions.md` · `logs/schema.md` · `ROADMAP.md`
