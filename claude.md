# CLAUDE.md
> Session router — load only what the task requires.
> See [`conventions.md`](conventions.md) for documentation standards.
> Last updated: 2026-06-05

---

## On load — always

1. Read `./logs/errors.md` → surface any OPEN flags immediately
2. Read `./logs/config.md` → verify current tool state
3. Route below; load nothing else unless the task requires it

---

## Routing table

| Task | Load |
|------|------|
| Scan output provided / FRST analysis | [`procedure.md`](procedure.md) §1–3 |
| Running a scan cycle (either mode) | [`procedure.md`](procedure.md) |
| Tool paths, versions, invocation | [`tools.md`](tools.md) |
| Writing or reading logs | [`logs/schema.md`](logs/schema.md) |
| Toolchain membership / inter-tool flow | [`apps/README.md`](apps/README.md) |
| Commit style / naming / documentation | [`conventions.md`](conventions.md) |
| Deferred or planned work | [`ROADMAP.md`](ROADMAP.md) |
| No specific task | Report open errors → offer Phase 0 |

---

## Escalation — halt immediately

- `<==== ATTENTION` FRST finding that is not a simple No-File orphan
- Active malware (running process/service with unknown signature)
- Multiple real-time AV engines conflicting
- Firewall disabled unexpectedly
- New unknown account since last scan
- C: free space < 5 GB
- Windows Update non-trivial failure
- BSOD / critical stop in event log

## Human gates

FIXLIST deployment · registry edits · any destructive system change

---

*Full operational detail lives in the routed files above. Do not load all at once.*
