---
name: spec
description: Turns an idea or an unrefined issue into an approved spec (goal, acceptance criteria, out of scope) and creates or updates the GitHub issue(s). Splits large tasks into vertical slices. Writes no code.
disable-model-invocation: true
model: opus
argument-hint: "[#issue | idea]"
---

# Spec

Input: $ARGUMENTS (an idea, or `#N` for an existing issue). I bring the requirements; you order and slice them.

## 1. Understand

- Read the project's CLAUDE.md and only the docs it points to for scope and domain. With `#N`: `gh issue view N --comments`.
- Read only the code needed to understand the current behavior. Don't design yet.
- Ask what's missing, at most 3 questions per round.
- Ask which decisions would be expensive to change later. Name them; don't design them.
- If the request falls outside the project's documented scope, stop and say so.

## 2. Propose

- **Goal:** one line, from the user's point of view.
- **Acceptance criteria:** Given/When/Then. What the user sees or gets, never how it's built. Number them (C1, C2…).
- **Split** when there are more than 5 criteria or more than one deliverable:
  - Each slice is vertical: it crosses every layer and delivers value on its own. Never split by layer.
  - No technical slices: infrastructure goes inside the first slice that needs it.
  - New project: slice 1 is the walking skeleton (the smallest thing usable end to end).
  - One issue per slice, in order. Add `Requires: #N` only when a slice makes no sense without another.
- **Out of scope:** what's left out, so it doesn't sneak in.
- **Open questions:** missing business data as `[TBD]`. Never invent it.

## 3. Approve and publish

- Show the issue body (or one per slice) and wait for my OK. We adjust until I say yes.
- Repo without a GitHub remote: stop and offer `/project-setup`.
- Create it with `gh issue create`, or update `#N` with `gh issue edit`. Labels and milestone only if the project's CLAUDE.md defines them.
- No personal data, commercial terms or credentials: issues travel with the repo.
- End with the issue link(s) and: "Next: /clear, then /test-list #N".

## Issue format

```markdown
## Goal
<one line>

## Acceptance criteria
- [ ] C1. Given <context>, when <action>, then <result>.
- [ ] C2. ...

## Out of scope
- ...

## Expensive decisions
- <or "none">

## Open questions
- [TBD] ...
```
