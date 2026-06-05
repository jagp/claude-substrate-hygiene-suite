# ROADMAP.md — Deferred and Planned Work

> Preserved intent, not implementation specs.
> Items here are stubs until explicitly promoted to active work.
> Last updated: 2026-06-05

---

## Automation Layer

### App Chain Orchestration (`./apps/`)
> *"run the appchain together (possibly in parallel w subagents?) as a unit"*

Location: `./apps/`  
Status: folder stubbed. Orchestration logic not yet designed.  
Candidate: parallel subagent execution per tool, results aggregated before analysis.

### Routing Table for Scans
> *"routing table for scans — expand that style project-wide including using stubs and references to on-load .md files being called only when token-cost-efficient"*
> *"Jeff Su / CoworkAcademy '5 Things I Wish I Knew' video — routing table MVP (link TBD)"*

Status: stub. Design pending reference link + review.  
Intent: lightweight dispatch layer — load only what's needed per scan mode, minimize token cost on shallow runs.

### Scan Cadence Optimization
> *"don't want to jam through a million tokens just for what should be relatively few actual needs after initial burst. Let's interrogate this — not positive it'll be the long-term system"*

Status: stub. Shallow/deep modes defined in `procedure.md`. Deeper optimization deferred.  
Working assumption: scripts run autonomously on schedule; I'm invoked only when something is flagged.

---

## Skills and Hooks

### `defer` Skill — Monolithic Change Order Handler
> *"skill to push back against monolithic change orders... query me when I'm unreasonably unclear. A 'defer' skill so I can still do a brain dump without you choking"*

Status: designed, pending build as installable Cowork skill.  
Behavior: when a prompt contains 4+ distinct concerns, triage them as IMMEDIATE / DESIGN / STUB / DEFER before acting. Present classification for confirmation.

---

## Dashboard / GUI Layer
> *"snazzy heads-up info panel artifact... structured to allow a GUI to be layered on top"*
> *"so far down the line it's nearly irrelevant — mention it in case there are structural changes worth clearing space for"*

Status: structural space cleared. `./logs/schema.md` defines the log format a parser would consume.  
No implementation planned until the rest of the stack stabilizes.

---

## Claude Code Migration

Switch from Cowork to Claude Code when:
- PowerShell execution is needed for actual scan runs (not just output analysis)
- The `/apps` orchestration layer moves from stub to implementation
- Pre-commit hook iteration requires persistent terminal sessions

Current Cowork coverage is sufficient for all documentation, analysis, and file work.
