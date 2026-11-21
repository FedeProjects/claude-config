---
name: tdd-close
description: Closes a task once its test list is done. Runs the full checks, sorts out Discovered items, records project-wide decisions and drafts the PR. Removes the local work file.
disable-model-invocation: true
model: sonnet
argument-hint: "[#issue]"
---

# Close

## 1. Check the list

- Read the work file. Every item is ticked, or I dropped it explicitly.
- For each "Discovered" item, propose one option: a new issue, a new item here (back to `/tdd`), or drop it. I decide.
- Every acceptance criterion in the issue is ticked. If one isn't, say which and why.

## 2. Verify

- Run the "full" checks from `## Checks`. Everything must be green; if not, fix it or tell me.
- Walk me through the `manual` items, one at a time.
- Suggest a review from a fresh context (`/clear`, then `/code-review`). Don't review your own code.

## 3. Record

- Project-wide decisions from "Decisions": propose the line for the project's docs (decisions record or CLAUDE.md). It goes in this same change.
- A recorded decision that changed: update that record.

## 4. PR

- Draft it:
  - Title in Conventional Commits.
  - Body: `Closes #N`, a short summary, the decisions, and "How to test" with runnable steps (each long-running command in its own terminal).
  - Show it and wait for my OK.
- After I push, create it with `gh pr create`.
- Tell me if the project doesn't merge with squash: the branch's commits will stay in the default branch's history.

## 5. Clean up

- Delete `.claude/work/<n>-<slug>.md`. If there's a "Handoff" comment on the issue, mention it so I can delete it.
- End with: "Next: /clear".
