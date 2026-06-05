# conventions.md — Documentation Standards

> Codifies tone, naming, and structural rules for all tracked files.
> Last updated: 2026-06-05

---

## Naming

| Instead of | Use |
|-----------|-----|
| "Digital Hygiene Suite" (repeated) | "the suite" — first reference in a doc may expand; subsequent refs shorten |
| "Claude" in operational docs | "I" or "me" for first-person actions; "we" for shared intent |
| Personal name | "user" — no personal identifiers in tracked files |
| Absolute user paths | `%USERPROFILE%\...` or `./` relative from project root |

Machine-specific values (usernames, hostnames, exact paths) live in `.env` only.

## Tone

- **Professional** — employer-facing at all times
- **Self-effacing humor** is welcome when raw user input (typos, slang, stream-of-consciousness) is being interpreted or transcribed; a light touch acknowledges it without dwelling
- **Active voice** — "I generate a FIXLIST" not "a FIXLIST is generated"
- **"We"** for shared decisions — "we prefer conservative FIXLIST inclusion"

## Decoding User Input

Aggressively decode slang and non-standard abbreviations before logging or documenting:
- Reconstruct intent from context, not literal text
- Correct obvious typos in any output derived from user input
- When genuinely ambiguous, note the source phrasing briefly

## Commit Style

Format: `type: short imperative description`

Types: `init` · `style` · `structure` · `procedure` · `schema` · `hooks` · `fix` · `docs` · `feat`

Split commits by logical concern. One large commit touching multiple areas is a smell.

Feature branches + PR when: the change is large or risky, involves a new tool integration, or touches procedure logic that warrants review before merge.

## Path References

- Documentation: `./relative/path` from project root
- Operational tools: `%USERPROFILE%\Downloads\ToolName.exe`
- Machine-specific overrides: `.env` (gitignored)
