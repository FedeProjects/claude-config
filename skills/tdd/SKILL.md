---
name: tdd
description: Runs the approved test list with TDD (red, green, refactor, commit), item after item, and stops only when it needs me. High-risk lists stop at each red for approval. "pause" hands the work off to another machine; "#N" resumes it from the issue.
disable-model-invocation: true
model: sonnet
argument-hint: "[item | N | pause | #issue]"
---

# TDD

You are the driver; I'm the navigator. I approved the list: that is the contract, so you don't need my OK for each item. Input: $ARGUMENTS.

- Empty: every unchecked item, in order, until the list is done or a stop condition is hit.
- An item: only that item.
- A number N: the next N items.
- `pause`: see "Pause".
- `#N`: resume. Use the local work file; if there's none, rebuild it from the issue's latest "Handoff" comment.

## Start

- Read `.claude/work/<n>-<slug>.md`. No file: offer `/test-list #N`.
- Read only the code and tests of the item, plus `## Checks` from the project's CLAUDE.md. Don't reread the issue unless the item is unclear.
- If you're not on the work file's branch, switch to it, or create it from the up-to-date default branch.
- If "Next step" has content, continue from there and clear it.
- Read the work file's `Risk`. **High:** keep CONTROL 1 (stop at each red for my OK). **Medium or low:** no stops; go on to the next item after each commit.

## Red

- Write only the test. Its name describes the behavior.
- Test through the public interface, not internal details.
- The expected value comes from the item, never computed with the same logic as the code.
- Doesn't compile: add the minimum signature, nothing else.
- Run it with the "one test" command. It must fail on the assertion, not on compile or setup.
- **CONTROL 1 (high risk only):** the relevant lines of the red output and one line with the decision it locks in. Don't paste the test: I read it in my editor's diff view. Explain a part of it only when it isn't obvious. Wait for my OK.
- Medium or low risk: check the red yourself (it fails on the assertion, for the reason the item describes) and go on.

## Test quality

- Assert on observable results (returned value, stored state, response, log, message sent), not on how the code gets there.
- Real collaborators by default. Test doubles only at the system's edges (external APIs, email, clock, randomness).
- `verify` / captors only when the interaction is the behavior (an email is sent, an external API is or isn't called).
- One behavior per test. Precise assertions: they must fail for the bug the item describes and only for it.
- Time and randomness are injected (`Clock`, seeds), never `Instant.now()` inside the logic under test.

## Green

- Write the minimum code for this test. Nothing the test doesn't require.
- Run the "fast" checks. If another test breaks, fix the code, never the test.
- Refactor only with everything green, after the green. If it's not worth it, say so in one line.
- Commit on green, without asking: Conventional Commits, `Refs #N` in the footer, only the item's files. Behavior and refactor go in separate commits.
- Local commits can be redone before the push: my real control is the PR text, which still needs my OK.

## Close the item

- Tick the item. Add new cases to "Discovered" without implementing them, and record decisions in "Decisions".
- Go on to the next item.

## Report

When the run ends (list done, N items done, or a stop):

- A table: item, red reason in a few words, commit hash.
- The checks run, with their result.
- What went to "Discovered" and "Decisions".
- The criteria the items completed. Ticking them in the issue publishes it: propose it and wait for my OK.
- If it stopped: why, and what you need from me.
- List complete: "Next: /tdd-close". Suggest `/clear` only if the context is past ~60%.

## Stop immediately (even mid-run) if

- You need a design decision that isn't in the issue or the work file.
- You think of adding something that wasn't asked for, even if it seems reasonable.
- Making a test pass would need changing, skipping or deleting a test.
- Two attempts without reaching green.
- The step exceeds ~50 lines or touches more than 3 files: propose splitting the item.

## Pause

Emergency exit to continue on another machine:

1. Fill "Next step" in the work file with exactly where we are.
2. Show the issue comment, wait for my OK and post it:
   ````markdown
   <details><summary>Handoff: test list</summary>

   ```markdown
   <full work file>
   ```
   </details>
   ````
3. Show a WIP commit (`test: add failing test for <item> (wip)` or similar) and its files. After my OK, commit and push the branch.
