# todo.md
> Active work queue. Updated in place — no versioning.

---

## immediate

- [ ] Windows cleanup before next commit:
  ```powershell
  Remove-Item -Recurse -Force FRST, HitmanPro, AdwCleaner, MalwareBytes, WinDefender
  Remove-Item apps\shallow.md, apps\deep.md
  Remove-Item test_write.txt
  ```
- [ ] Run 4 commit sequence + push (see below)
- [ ] Audit pass commit — dead references, consistency, best practice gaps

## commit queue

```powershell
# Commit 1
git add .gitignore .env.example README.md
git commit -m "init: gitignore, env template, readme stub

Co-authored-by: Claude <claude-ai@anthropic.com>"

# Commit 2
git add CLAUDE.md conventions.md procedure.md tools.md
git commit -m "style: routing table, conventions, rewrite core docs

Co-authored-by: Claude <claude-ai@anthropic.com>"

# Commit 3
git add apps/ logs/schema.md ROADMAP.md
git commit -m "refactor: toolchain under apps/, lowercase names, update all refs

Co-authored-by: Claude <claude-ai@anthropic.com>"

# Commit 4
git add .githooks/
git commit -m "hooks: pre-commit codespell typo check

Co-authored-by: Claude <claude-ai@anthropic.com>"

# Wire remote and push
git remote add origin https://github.com/jagp/claude-substrate-hygiene-suite
git branch -m master main
git push -u origin main
```

## backlog

- [ ] `defer` skill — build as installable Cowork skill (design in ROADMAP.md)
- [ ] Jeff Su routing table reference — link TBD, expand methodology to logs/schema.md
- [ ] Scan cadence decision — shallow weekly / deep monthly confirmed? (ROADMAP stub)
- [ ] Claude Code migration — trigger: when PowerShell execution needed for live scans

---
*Move completed items to git commit messages, not here.*
