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
- Suggest one review from a fresh context (`/clear`, then `/code-review`). Don't review your own code.
- Triage its findings with one rule, so the review ends:
  - **Fix now:** only Important findings that this change introduced. With a test first, like any bug.
  - **Already there before this change:** a new issue, or a line in the issue that will touch that code.
  - **Nits and "what if, some day":** a line in the issue that will touch that code, or drop them. Never a new round here.
- After the fixes, at most one more review, of the new commits only. Never the whole branch again.

## 3. Record

- Project-wide decisions from "Decisions": propose the line for the project's docs (decisions record or CLAUDE.md). It goes in this same change.
- A recorded decision that changed: update that record.

## 4. PR

- Draft it:
  - Title in Conventional Commits.
  - Body: `Closes #N`, a short summary, the decisions, and "How to test" with runnable steps (each long-running command in its own terminal).
  - Show it and wait for my OK.
- After my OK, push the branch and create it with `gh pr create`. Never merge: I merge.
- Tell me if the project doesn't merge with squash: the branch's commits will stay in the default branch's history.

## 5. Clean up

- Delete `.claude/work/<n>-<slug>.md`. If there's a "Handoff" comment on the issue, mention it so I can delete it.
- End with: "Next: /clear" (the task is over).
