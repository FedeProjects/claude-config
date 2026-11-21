# claude-config

User-level Claude Code config: global `CLAUDE.md` and the workflow skills (`/spec`, `/test-list`, `/tdd`, `/tdd-close`, `/project-setup`).

This repo is cloned **into** `~/.claude` (WSL home). `.gitignore` keeps credentials, sessions and synced skills out.

## Install on a machine (WSL)

```bash
cd ~/.claude
git init -b main
git remote add origin git@github.com:FedeProjects/claude-config.git   # or the https URL
git fetch origin
git checkout -f -t origin/main   # overwrites the local CLAUDE.md with the repo's version
```

Update: `cd ~/.claude && git pull`.

## Dev Container mounts

Add to each project's `.devcontainer/devcontainer.json`, then rebuild the container:

```json
"mounts": [
  "source=claude-code-config-${devcontainerId},target=/home/vscode/.claude,type=volume",
  "source=${localEnv:HOME}/.claude/CLAUDE.md,target=/home/vscode/.claude/CLAUDE.md,type=bind,readonly",
  "source=${localEnv:HOME}/.claude/skills,target=/home/vscode/.claude/skills,type=bind"
]
```

`~/.claude/skills` must exist in WSL before the rebuild; otherwise Docker creates it empty and owned by root.
