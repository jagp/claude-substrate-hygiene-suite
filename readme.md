# claude-substrate-hygiene-suite

> Autonomous PC security and maintenance, coordinated through Claude.

A structured, AI-assisted workflow for keeping a Windows machine clean and auditable. Combines forensic scanning (FRST), malware detection (HitmanPro, MalwareBytes, AdwCleaner), and Windows Defender health checks into a coherent, logged, reviewable process.

## What this is

- Procedural documentation and run algorithms for a multi-tool security sweep
- Structured log formats designed for both human review and eventual tooling
- A practical example of AI-assisted system maintenance with clear human-in-the-loop checkpoints — the AI analyzes and generates; the human reviews and deploys

## Status

Active development. Initial scan cycle complete; remediation in progress.

## Structure

| File | Purpose |
|------|---------|
| [`CLAUDE.md`](CLAUDE.md) | Session anchor — loaded automatically each session |
| [`procedure.md`](procedure.md) | Full run algorithm (scan modes, phases, escalation) |
| [`tools.md`](tools.md) | Software inventory and invocation details |
| [`conventions.md`](conventions.md) | Documentation standards |
| [`ROADMAP.md`](ROADMAP.md) | Deferred and planned work |

## Notes

Runtime data (scan logs, FRST archives, diagnostic output) is gitignored — this repo tracks structure and procedure, not machine state. Machine-specific values live in `.env` (see `.env.example`).
