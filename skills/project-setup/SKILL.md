---
name: project-setup
description: Complements /init for this workflow. Adds the CLAUDE.md sections the skills rely on (Checks, Tests, Git & GitHub), and sets up the test runner, the GitHub repo and the work folder when missing.
disable-model-invocation: true
model: opus
---

# Project setup

Run after `/init`. Add only what's missing; never rewrite what's there.

## 1. Read

- No CLAUDE.md: stop and tell me to run `/init` first.
- Read CLAUDE.md, the build and CI files, `.gitignore`, and one test per layer if there are any. Don't explore the whole repo.
- Check `git remote -v` and `gh auth status`.
- Ask only what you can't deduce (merge strategy, labels, the test levels I want).

## 2. Propose the sections that are missing

```markdown
## Checks
- One test: `<command with a placeholder for the test>`
- Fast (on every green): `<command>`
- Full (before the PR): `<command>`

## Tests
- Runner and location. Levels in use (unit, integration, e2e) and which ones are slow.

## Git & GitHub
- Branches: `<type>/<issue>-<slug>`. PRs: Conventional Commits title, `Closes #N`.
- Merge: <squash | merge>. Labels and milestones: <list or "none">.
```

Also add "What not to touch" and confusing domain terms, but only if `/init` left them out and they don't come out of reading the code.

## 3. Fill the gaps (each one needs my OK)

- No test runner: propose one for the stack, with its "one test" command, or record in CLAUDE.md that it gets added with the first task that has logic. Adding it is a new dependency.
- No GitHub repo: propose `gh repo create <name> --private --source .`, without pushing. I push.
- `.claude/work/` missing from `.gitignore`: add it.

## 4. Apply

- Show the full diff and wait for my OK.
- Apply it and propose the commit message (`chore: set up AI workflow` or similar). I commit.
