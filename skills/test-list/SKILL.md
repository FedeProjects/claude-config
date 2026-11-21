---
name: test-list
description: Builds the test list for one GitHub issue using the ZOMBIES heuristic and saves it to a local work file. Writes no code.
disable-model-invocation: true
model: opus
argument-hint: "#issue"
---

# Test list

Input: $ARGUMENTS (`#N`). The list is the contract for `/tdd`: I approve it, not you.

## 1. Read

- The issue: `gh issue view N`. No refined acceptance criteria: stop and offer `/spec #N`.
- The project's CLAUDE.md, especially `## Checks` and `## Tests`.
- The code the issue touches and its tests. Nothing else.
- Project rules whose `paths` match the touched files (`.claude/rules/`). They may require specific tests; include them.
- An open question in the issue that blocks a test: ask before proposing.

## 2. Propose

ZOMBIES: Zero, One, Many set the order (simple to complex). Check Boundaries, Interface and Exceptions at each step. Simple: the simplest scenario and solution first.

- One line per item: `situation → expected result`, its ZOMBIES letter, its level, the criterion it covers, and a short "why" when it's not obvious (it teaches how to think about tests).
- Level: the highest one that stays fast and deterministic. `unit | integration | e2e | manual`. For `manual`, say how it's verified.
- Every acceptance criterion is covered by at least one item.
- The expected result comes from the criterion. If it isn't defined, it goes to "Questions"; never invent it.
- The code to change has no tests: the first items are characterization tests that pin down what it does today.

## 3. Save and stop

- Save to `.claude/work/<n>-<slug>.md`. If `.claude/work/` isn't in `.gitignore`, add it and tell me.
- Wait for my approval. On OK, end with: "Next: /clear, then /tdd".

## Work file format

```markdown
# #<n> <issue title>
Issue: <url>
Branch: <type>/<n>-<slug>

## Decisions
- <decision> — <why>

## Questions
- ...

## Test list
- [ ] [Z] empty cart → total is 0 · unit · C1
- [ ] [B] order exactly at the free-shipping threshold → free shipping · integration · C3 — the boundary is inclusive

## Discovered
- <new case, not approved yet>

## Next step
- <only when paused mid-item>
```
