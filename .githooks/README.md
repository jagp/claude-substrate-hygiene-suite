# .githooks/

Custom git hooks for this project.

## Activation

```bash
git config core.hooksPath .githooks
chmod +x .githooks/pre-commit
```

## Hooks

### pre-commit
Runs `codespell` on all staged `.md` files. Blocks commit if typos are found.

```bash
pip install codespell        # one-time setup
codespell -w <file>          # auto-fix a file
```
