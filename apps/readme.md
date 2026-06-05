# apps/ — Security Toolchain

The integrated set of third-party tools that collectively produce the security maintenance outcome.

## Toolchain Members

| Tool | Folder | Role |
|------|--------|------|
| FRST64 | `./frst/` | System forensic scan + repair engine |
| HitmanPro | `./hitmanpro/` | Cloud behavioral malware (second opinion) |
| AdwCleaner | `./adwcleaner/` | Adware / PUP / browser hijacker removal |
| MalwareBytes | `./malwarebytes/` | On-demand + real-time malware detection |
| 360 Total Security | — | Supplemental AV + vulnerability scan (no archive) |
| Windows Defender | `./windefender/` | OS-native real-time AV + firewall (baseline) |

## Inter-tool Notes

- FRST runs first and last — forensic baseline, then post-fix verification
- HitmanPro and MalwareBytes are independent second opinions
- AdwCleaner targets adware/PUPs — different threat class than the AV tools
- Defender is always the baseline; others supplement, never replace it
- 360 TS is supplemental — disable its real-time protection if it conflicts with Defender

For execution workflow and scan modes, see [`../procedure.md`](../procedure.md).
