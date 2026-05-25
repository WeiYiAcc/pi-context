---
name: context-management
description: Read this skill for work that is likely to stretch across many turns or get messy if handled in one uninterrupted thread. Especially use it when the request sounds like first go search or read a lot, first look through logs/files/pages/results and come back with the real conclusion, this will probably take several rounds, there may be several approaches to try, first make a plan or todo list and then work through it, there will be many similar cases to handle, the user may interrupt with side tasks, or the thread is already getting messy and should be cleaned up before continuing. It teaches a working mode built around frequent checkpoints, periodic timeline review, and targeted rewinds as the task evolves. Usually skip it for one-shot reads, bounded summaries, direct rewrites, or deterministic scripts.
---

# Context Management

Use this skill to adopt an explicit conversation-management working mode.

This mode is for tasks where the work may still be valid, but the thread carrying it is likely to become long, noisy, multi-phase, or hard to keep clean. The goal is to keep the work navigable: checkpoint early, review timeline when needed, and compact completed noisy phases back to clean anchors at useful continuation boundaries.

Use only the current tool names:
- `context_checkpoint`
- `context_timeline`
- `context_rewind`

## When to use this skill

Read this skill when the task is likely to outgrow one clean thread and would benefit from explicit history management.

It should feel like a good fit when the user is effectively asking for one of these patterns:

### “First go look through a lot of stuff, then come back with the real conclusion.”
Examples:
- searching or researching first
- web search or browser-driven information gathering
- reading many files, logs, docs, webpages, or web results
- review / comparison / audit-heavy reading across many sources

### “This will probably take several rounds or several phases.”
Examples:
- investigate -> decide -> execute -> validate
- plan -> implement -> verify
- collect -> compare -> conclude
- any task that will keep changing shape as it progresses

### “We may need to try more than one route.”
Examples:
- A / B / C approaches
- dead ends and retries
- compare and choose
- pivoting after a failed direction

### “Start from a plan or todo list, then work through it.”
Examples:
- a formal implementation or migration plan
- a roadmap with milestones
- a lightweight todo list or checklist
- staged execution that will be revisited across many turns

### “There will be many similar cases, tickets, or items.”
Examples:
- repeated cases
- repeated tickets
- repeated checks
- repeated reviews
- repeated fix attempts on similar inputs

### “I may interrupt you, or this thread is already getting messy.”
Examples:
- the user may insert side tasks
- you need to pause one line of work and resume it later
- the thread is already long, stale, or cluttered
- finished noisy phases should be cleaned up before continuing

### “This is code-facing work and may turn noisy while I debug or implement it.”
Examples:
- implementation
- debugging
- troubleshooting
- refactoring
- migration
- code-facing review or fix work

If one of these patterns clearly applies, take the first structural action now—usually a checkpoint—instead of only describing the intended workflow.

If multiple approaches, failed branches, compare work, or pivots become central, also read `references/retry-branch-and-pivot.md` in addition to the main scenario reference.

## When to skip it

Usually skip this skill for:
- one-shot reads
- bounded summaries
- direct rewrites with little exploration
- simple fact lookup or concept explanation
- deterministic scripts or fixed-rule automation
- short tasks that can stay clean in one thread

## Operating rhythm

### Start-of-turn check
At the start of each new user message, quickly classify the relationship to the previous active segment:
- **Same task / next phase**: continue; rewind first if the previous phase is complete and noisy enough to compact.
- **Correction or follow-up on the just-finished answer**: usually do not rewind yet; answer using the recent context.
- **New unrelated task**: if the previous task was noisy and complete, rewind to its clean anchor with a summary before starting the new task.

This start-of-turn check is what prevents both failure modes: it avoids premature rewinds immediately after a final answer, and it also prevents endless checkpoint-only behavior across real task switches.

### Main loop
1. Before entering a noisy phase, create a checkpoint.
2. If you feel disoriented or anchor choice is unclear, run `context_timeline`.
3. Add more checkpoints at milestones, phase boundaries, risky branches, and interruptions.
4. Do not rewind just because the skill triggered, and do not rewind only because a user-visible task has just ended.
5. Rewind only when there is an immediate continuation that benefits from cleanup: the next phase of the same task, the next repeated item, the next branch, or a later user message that starts a different task.
6. If the whole requested task is complete and your only remaining action is to answer the user, answer cleanly and wait. On the next user message, decide whether the previous noisy segment should be compacted before starting new work.
7. Continue from the compacted state instead of dragging the full messy path forward.

The key habit is: **checkpoint is the default move; rewind is the selective cleanup move.** Checkpoints create anchors so that completed noisy phases can later be compacted cleanly. Checkpointing alone is not the end state across long or multi-task conversations, but a final response after a completed task is not itself a reason to rewind.

## Tool policy

### `context_checkpoint`
This is the default tool in this mode.

Use it:
- before noisy work
- before a new phase
- after a meaningful milestone
- before a risky attempt
- before switching to another subtask

Use semantic names so the timeline stays readable.

Recommended patterns:
- `<task>-start`
- `<task>-<phase>`
- `<task>-<attempt>`
- `<task>-<milestone>`

Examples:
- `auth-oauth-start`
- `timeout-analysis-search`
- `db-migration-plan`
- `parser-fix-attempt-2`
- `vendor-review-milestone-1`

Avoid generic names like `start`, `checkpoint-1`, `phase-1`, or `retry`.

### `context_timeline`
Use this tool to regain orientation.

Use it:
- when you feel lost or drifted
- when several checkpoints or branches now exist
- before choosing a rewind target
- at major phase transitions if you need a quick map
- when the thread feels cluttered and you want a structural view before acting

When you inspect timeline, ask:
- Where is the nearest clean anchor?
- Has the current segment grown too long without a clean checkpoint?
- Am I carrying a failed or stale branch forward?
- Is there already a better checkpoint to rewind to?

### `context_rewind`
Use this tool to compact a path that should no longer stay active in full.

Use it:
- after a noisy investigation has produced a stable finding and there is more work to do from that finding
- after a failed attempt has produced a clear lesson and the next attempt should start cleanly
- after a completed phase where the next step is clear
- after a representative item or branch has already taught you what you need
- before starting a new, different user task when the previous completed task left noisy but summarizable history
- when the current path is dragging too much stale or low-value context forward

Do not use it as an automatic epilogue after finishing a user request. A successful use of this mode often includes `context_rewind` at continuation boundaries, not just more checkpointing, but the right boundary may be the next user message rather than the current turn.

## Rewind protocol

### Rewind gate
Before using `context_rewind`, require all three conditions:
1. The segment being left behind is noisy, stale, failed, or low-value in raw form.
2. You can summarize the useful result, lesson, or state clearly enough for future-you.
3. There is an immediate continuation that benefits from a cleaner context.

If conditions 1 and 2 are true but condition 3 is not—because the entire requested task is done and you are about to give the final answer—wait. Let the next user message reveal whether this is a continuation, a correction, or a new task. If it is a new task, rewind before starting that new task.

### When to rewind
Rewind when the current phase is ready to be compacted and a clean continuation is useful now.

Good reasons to rewind:
- a phase produced a stable finding or conclusion and the same task has another phase to run
- a failed path produced a clear dead-end lesson and you are about to try another path
- a milestone was reached and you want a cleaner continuation
- a representative item or branch has already taught you the reusable lesson
- a new user message starts a different task, while the previous completed task left noisy but summarizable history
- the thread is carrying more stale intermediate process than active value

A phase is usually rewind-ready when you now have one of these:
- a root cause or research conclusion
- a clear failure reason for abandoning a branch
- a completed implementation or troubleshooting phase
- a reusable method learned from a representative item
- a finished repeated-item segment whose raw path no longer needs to stay live

Do **not** rewind yet when:
- you are still in the middle of active exploration
- the result is still unstable or incomplete
- you only feel mild clutter and a checkpoint would be enough
- the current user request is complete and there is no new task or next phase yet
- you have not yet decided which earlier anchor you want to continue from

When in doubt: checkpoint first, review timeline if needed, and rewind later once the phase boundary or task switch is clearer.

### Choose the target and backup
Choose the **best earlier clean anchor**, not just any earlier checkpoint.

Usually prefer:
- the start of the noisy phase you are compacting
- the nearest clean phase-start before the current failed or completed path
- the repeated-work anchor for repeated-item workflows
- the last stable pre-branch checkpoint when abandoning an approach

Avoid:
- rewinding too far back when a nearer clean anchor exists
- rewinding to a checkpoint that still includes the noise you want to leave behind
- defaulting to `root` unless the whole active path should be reset

If you are unsure which anchor is best, run `context_timeline` first.

Use `backupCheckpoint` when:
- the raw path may still be useful later
- you are compacting a long investigation
- you are abandoning a branch but may need its details again
- you want a safe recovery point before a major rewind

A backup checkpoint preserves the raw path you are compacting. If the summary later proves insufficient, return to that backup instead of redoing the whole investigation.

If the raw path is low value and unlikely to matter again, a backup is optional.

### Write the message
A rewind message is the baton pass to future-you. It is not a throwaway comment.

At minimum, include:
- the main result, lesson, or failure summary
- why you are rewinding now
- important changes, especially changed files if disk state changed
- the next step after the rewind

Useful shapes:
- `[result] + [reason for rewind] + [next step]`
- `[result] + [reason for rewind] + [important decision or change] + [next step]`
- `[result] + [reason for rewind] + [important changes / changed files] + [next step]`

Good examples:
- `Found DB connection pool exhaustion as the likely root cause. Reason: investigation phase is complete and ready to compact. Next step: report findings and propose mitigation.`
- `Parser fix is implemented and the debugging phase is complete. Reason: compacting implementation history before focused validation. Important changes: modified src/parser.ts and test/parser.test.ts. Next step: run targeted validation and summarize remaining edge cases.`
- `WeakRef approach failed because objects were collected too early and cache hit rate collapsed. Reason: abandoning this attempt and returning to a clean anchor. Next step: try object pooling instead.`

Avoid vague messages like:
- `Done`
- `Switching context`
- `Investigated`
- `Going back`

Before using `context_rewind`, quickly check:
- Is there already a stable result, lesson, or failure summary?
- Is there a real continuation now, rather than just cleanup after a final answer?
- Do I know why this is the right moment to rewind?
- If disk state changed, did I record the important changes or changed files?
- Is the next step explicit?
- Would future-me understand what changed just from this message?

If not, keep working a bit longer or checkpoint first.

### After rewind
When `context_rewind` succeeds:
1. Read the injected summary carefully.
2. Treat it as the new active state.
3. Execute the next step from that summary.
4. Do not keep reasoning from the full old path unless you intentionally return to a backup checkpoint.

## Common mistakes

Avoid:
- checkpointing constantly without phase meaning
- rewinding blindly without checking timeline when anchor choice is unclear
- rewinding immediately after a final deliverable when no next user intent is known
- carrying completed noisy phases across a task switch instead of compacting them before the new task
- writing vague rewind messages that future-you cannot act on

## Read the right reference

Read **one primary reference** based on the task shape:
- search / research / reading-heavy work, especially web search, browser operation, or low-density webpage reading -> `references/search-research-and-reading.md`
- development / debugging / troubleshooting / refactoring / migration -> `references/development-and-troubleshooting.md`
- planning / staged execution / todo-driven work -> `references/planning-and-execution.md`
- repeated similar items / batch work -> `references/repeated-items-and-batch-work.md`
- task switching / pause-resume / cleanup-and-continue work -> `references/task-switching-and-cleanup.md`

Then read the cross-cutting retry reference when needed:
- multiple approaches, failed branches, compare, retry, or pivot behavior -> `references/retry-branch-and-pivot.md`

Keep the mode simple:
- checkpoint before mess
- review timeline when orientation matters
- rewind when a phase is ready to be compacted
