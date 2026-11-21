---
name: tdd
description: One TDD iteration (red, green, refactor) on the next item of the local work file, stopping for approval at each control. "pause" hands the work off to another machine; "#N" resumes it from the issue.
disable-model-invocation: true
model: sonnet
argument-hint: "[item | N | pause | #issue]"
---

# TDD

You are the driver; I'm the navigator. Input: $ARGUMENTS.

- Empty: the first unchecked item.
- An item: that item.
- A number N: N full cycles. Show all the tests first, then the code.
- `pause`: see "Pause".
- `#N`: resume. Use the local work file; if there's none, rebuild it from the issue's latest "Handoff" comment.

## Start

- Read `.claude/work/<n>-<slug>.md`. No file: offer `/test-list #N`.
- Read only the code and tests of the item, plus `## Checks` from the project's CLAUDE.md. Don't reread the issue unless the item is unclear.
- If you're not on the work file's branch, switch to it, or create it from the up-to-date default branch.
- If "Next step" has content, continue from there and clear it.

## Red

- Write only the test. Its name describes the behavior.
- Test through the public interface, not internal details.
- The expected value comes from the item, never computed with the same logic as the code.
- Doesn't compile: add the minimum signature, nothing else.
- Run it with the "one test" command. It must fail on the assertion, not on compile or setup.
- **CONTROL 1:** the test, the relevant lines of the red output, and one line with the decision it locks in. Wait for my OK.

## Green

- Write the minimum code for this test. Nothing the test doesn't require.
- Run the "fast" checks. If another test breaks, fix the code, never the test.
- Refactor only with everything green, after the green. If it's not worth it, say so in one line.
- **CONTROL 2:** the diff and the proposed commits (message and files). Behavior and refactor go in separate commits. If the item completes a criterion, also propose ticking it in the issue. Wait for my OK, then commit.

## Close the item

- Tick the item. Add new cases to "Discovered" without implementing them, and record decisions in "Decisions".
- Tick the criterion in the issue (`gh issue edit`) if I approved it.
- End with one line naming the next item; don't start it. If the context is past ~50%, suggest `/clear`.
- List complete: "Next: /clear, then /tdd-close".

## Stop immediately (even mid-batch) if

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
