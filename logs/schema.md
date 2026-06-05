# logs/schema.md — Log Structure Specification

> Defines the format for all runtime log files.
> Log files themselves are gitignored (machine-specific data).
> This schema is tracked — it defines the structure for any future tooling or GUI layer.
> Last updated: 2026-06-05

---

## File Types

### errors.md — Persistent Flag Log

Single file. Append new items; prune resolved to `## Resolved` section.

```
# errors.md

## OPEN FLAGS

### [SEVERITY] Short Title
**Flagged:** YYYY-MM-DD
**Status:** OPEN — [action summary]
**Source:** [tool and section]
**Evidence:**
[raw output excerpt]
**What this means:** [plain-English explanation]
**Required action:**
1. Step one
2. Step two

---

## RESOLVED FLAGS
_(items moved here with resolution date and method)_
```

Severity: `CRITICAL` · `HIGH` · `MEDIUM` · `LOW` · `INFO`

---

### [YYYY-MM-DD]-activity.md — Daily Run Log

One file per session day. Scannable in under 30 seconds.

```
# YYYY-MM-DD Activity Log

## Summary
One-line: what ran and outcome.

## Actions Taken
- [tool/phase] description — outcome
- [tool/phase] description — outcome

## Flags Raised
- [SEVERITY] description → logged to errors.md

## Next Steps
- [ ] follow-up item
```

---

### config.md — Tool and Config State

Single file, no versioning. Updated in place.

```
# config.md

## Machine Profile
[hardware/OS snapshot table]

## Security Tool Registry
[per-tool: version, path, status, notes]

## Scan Cadence
[schedule stub]

## Known Deviations
[active non-standard configurations]
```

---

## GUI Layer Notes

When a dashboard is built, it will parse:
- `errors.md` — severity counts, OPEN flag list, last-updated timestamp
- `[latest]-activity.md` — last run summary, action count
- `config.md` — tool versions, machine profile

Structured for parsing via predictable Markdown heading patterns.
See `ROADMAP.md` for dashboard planning status.
