# apps/ — Security Toolchain

The integrated set of third-party tools that collectively produce the security maintenance outcome. Each tool plays a defined role; together they cover the full surface.

## Toolchain Members

| Tool | Role | Output |
|------|------|--------|
| FRST64 | System forensic scan + repair engine | `FRST.txt`, `Addition.txt`, `Shortcut.txt`, `Fixlog.txt` |
| HitmanPro | Cloud behavioral malware (second opinion) | Scan report, quarantine log |
| AdwCleaner | Adware / PUP / browser hijacker removal | Clean report |
| MalwareBytes | On-demand + real-time malware detection | Threat log |
| 360 Total Security | Supplemental AV + vulnerability scan | Scan report |
| Windows Defender | OS-native real-time AV + firewall (baseline) | MpThreat log, event log |

## Inter-tool Notes

- FRST runs first and last — initial forensic baseline, then post-fix verification
- HitmanPro and MalwareBytes are independent second opinions; neither depends on the other
- AdwCleaner targets a different threat class (PUPs/adware) than the AV tools
- Defender is always the baseline; other tools supplement, never replace it
- 360 TS is supplemental — disable its real-time protection if it conflicts with Defender

## Scan archive folders

Each tool stores its own output in a sibling folder at project root:
`../FRST/` · `../HitmanPro/` · `../AdwCleaner/` · `../MalwareBytes/` · `../WinDefender/`

For execution workflow (scan modes, phases, sequencing), see [`../procedure.md`](../procedure.md).
