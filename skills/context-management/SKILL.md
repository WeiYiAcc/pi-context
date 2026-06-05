---
name: context-management
description: Use this skill for work likely to span many turns, branches, retries, noisy research, reading several links/webpages, plan-then-execute phases, repeated cases, debugging, or interrupted/messy threads. It teaches explicit history management with checkpoints, timeline review when orientation matters, and compaction only at clean continuation boundaries. Usually skip for one-shot reads, bounded summaries, direct rewrites, simple lookups, or deterministic scripts.
---

# Context Management

Use this skill to keep long or messy work navigable. The core rhythm is:

- **checkpoint before mess**
- **review timeline when orientation matters**
- **compact only when a phase is ready and another phase benefits from cleanup**

Use only these tools:

- `context_checkpoint`
- `context_timeline`
- `context_compact`

## When to use

Use this mode when the user is asking for work that may outgrow one clean thread:

- search, research, browser work, or reading many files/logs/pages/results
- several links or low-density webpages where only conclusions and next actions should survive
- investigate -> decide -> execute -> validate
- plan -> implement -> verify
- multiple approaches, retries, failed branches, or pivots
- repeated similar cases, tickets, reviews, or batch items
- a main task that may be interrupted by side tasks
- a scattered thread that should be cleaned up before continuing from a clean point
- debugging, troubleshooting, refactoring, migration, or other code-facing work that may get noisy

If one of these clearly applies, take a structural action now, usually a checkpoint. Do not merely describe the workflow. If the user has not yet provided enough task details, still create a checkpoint for the workflow shape before asking clarifying questions, and name the detected mode briefly (search/reading, planning, repeated batch, task switching/cleanup, development retry).

Usually skip this skill for one-shot reads, bounded summaries, direct rewrites, simple fact lookup, conceptual explanation, deterministic scripts, or short tasks that can stay clean.

## Start-of-turn check

At the start of each new user message, classify it:

- **Same task / next phase**: continue; if the previous phase is complete and noisy, compact before the next phase.
- **Correction or follow-up on the last answer**: usually answer from recent context; do not compact yet.
- **New unrelated task**: if the previous task left a complete noisy segment, compact it to a clean anchor before starting the new task.

This prevents both premature cleanup after final answers and endless checkpoint-only behavior across real task switches.

## Main loop

1. Before noisy work, create a semantic checkpoint, even if the next visible step is asking for missing inputs.
2. When the task shape is clear, read the one matching scenario reference in the same turn; do not stop after checkpoint-only unless the prompt is intentionally lightweight.
3. If you feel lost, the thread has branches, or the compact target is unclear, use `context_timeline`.
4. Add checkpoints at milestones, phase boundaries, risky attempts, and interruptions.
5. Compact only when a completed or failed phase has a stable takeaway and there is an immediate continuation that benefits from a shorter context.
6. If the whole requested task is complete and your only remaining action is the final response, answer and wait. Let the next user message determine whether cleanup is useful.
7. After a successful compact, continue from the injected summary instead of dragging the full raw path forward.

### Phase-boundary reflex

After any checkpointed noisy phase, pause before the next tool call or next phase:

- Did this phase produce a stable result, lesson, or failure reason?
- Is the next action a new phase rather than the final answer?
- Would that next phase work from a compact summary instead of the full raw trail?

If yes, compact now instead of creating another checkpoint and dragging the raw path forward.

## Tool policy

### `context_checkpoint`

Default move. Use it before noisy work, a new phase, a risky attempt, switching subtasks, or after a meaningful milestone.

Use semantic names so the timeline stays readable:

- `<task>-start`
- `<task>-<phase>`
- `<task>-<attempt>`
- `<task>-<milestone>`

Examples: `auth-oauth-start`, `timeout-analysis-search`, `db-migration-plan`, `parser-fix-attempt-2`.

Avoid generic names like `start`, `checkpoint-1`, `phase-1`, or `retry`.

### `context_timeline`

Use it to regain orientation:

- when you feel disoriented or drifted
- when several checkpoints or branches exist
- before choosing a compact target
- when the thread feels cluttered and you need a structural map

When reading the timeline, ask:

- Where is the nearest clean anchor?
- Has the current segment grown too long without a checkpoint?
- Am I carrying a failed or stale branch forward?
- Is there a better checkpoint to compact to?

### `context_compact`

Use it to leave behind raw history that is no longer worth carrying in full.

Good times to compact:

- a noisy investigation reached a stable finding and the same task has another phase
- a failed path produced a clear lesson and you are about to try another path
- a completed phase is ready for validation, implementation, export, or the next item
- a representative repeated item taught the reusable method
- a new user task begins after a completed noisy previous task

Do **not** compact:

- while exploration is still active
- when the result is unstable or incomplete
- just because the skill triggered
- just because the user-visible task ended
- when you do not know which earlier clean anchor to continue from

A same-request phase transition is often the right time to compact: investigation -> execution, search -> export, diagnosis -> fix, representative item -> remaining batch.

## Compact gate

Before calling `context_compact`, require all three:

1. The segment being left behind is noisy, stale, failed, or low-value in raw form.
2. You can summarize the useful result, lesson, or state clearly.
3. There is an immediate continuation that benefits from cleaner context.

Condition 3 means the next action is a new phase, not just more of the same exploration. Examples: run the export, implement the fix, validate, process the next item, or try the chosen next approach.

If conditions 1 and 2 are true but the whole task is done and only the final answer remains, wait. Compact later only if the next user message makes it useful.

## Choosing target and backup

Choose the best earlier **clean anchor**, not just any checkpoint.

Prefer:

- the start of the noisy phase being compacted
- the nearest clean phase-start before the current failed or completed path
- the repeated-work anchor for batch workflows
- the last stable pre-branch checkpoint when abandoning an approach

Avoid compacting too far back, compacting to an anchor that already includes the noise, or defaulting to `root` unless the whole active path should reset. If you are unsure which anchor is best, run `context_timeline` first.

Use `backupCheckpoint` when the raw path may still matter later: long investigations, abandoned branches, risky compactions, or details that may be needed for recovery. A backup checkpoint is a recovery safety net, not a substitute for the summary; include details likely needed in the next phase because returning to backup is costly.

## Compact summary requirements

The summary is the handoff to future-you. It must preserve the compressed working set, not just the headline.

Always include:

- stable result, lesson, or failure reason
- why compacting is appropriate now
- important changes, especially changed files if disk state changed
- explicit next step

For search/research/reading-heavy work, also include likely-needed details:

- source anchors: files, URLs, session IDs, commands, queries, docs, records
- evidence: key facts, numbers, errors, IDs, or observations supporting the conclusion
- decisions and assumptions
- rejected leads that would be costly to repeat
- open questions or risks
- backup checkpoint name and when to use it

Useful shapes:

- `[result] + [why compact] + [next step]`
- `[result] + [evidence/source anchors] + [next step]`
- `[failure reason] + [lesson] + [next attempt]`
- `[changes] + [validation next step] + [backup pointer]`

Good examples:

- `Found DB pool exhaustion as likely root cause. Evidence: logs show pool wait timeouts during peak traffic; config has pool size 10; no network errors found. Reason: investigation is complete and ready for mitigation planning. Next step: propose fixes and validation.`
- `Parser fix is implemented. Important changes: src/parser.ts and test/parser.test.ts. Reason: compacting noisy implementation history before focused validation. Next step: run targeted tests and summarize remaining edge cases.`

Before compacting, quickly check: stable result? real continuation? right anchor? changed files captured? explicit next step?

Avoid vague summaries like `Done`, `Investigated`, `Switching context`, or `Going back`.

## After compact

1. Read the injected summary carefully.
2. Treat it as the new active state.
3. Execute the next step from that summary.
4. Return to the backup checkpoint only if the summary is insufficient.

## Common mistakes

Avoid:

- checkpointing constantly without phase meaning
- compacting blindly without timeline when anchor choice is unclear
- compacting immediately after a final deliverable when no next user intent is known
- carrying completed noisy phases into a new task
- writing summaries that omit evidence, decisions, changed files, or next step

## Read the right reference

Read **one primary reference** based on task shape whenever this skill is actively used and the shape is clear:

- search / research / reading-heavy work, especially web search, browser operation, or low-density webpages -> `references/search-research-and-reading.md`
- development / debugging / troubleshooting / refactoring / migration -> `references/development-and-troubleshooting.md`
- planning / staged execution / todo-driven work -> `references/planning-and-execution.md`
- repeated similar items / batch work -> `references/repeated-items-and-batch-work.md`
- task switching / pause-resume / interruptions to a mainline task / scattered-thread cleanup-and-continue -> `references/task-switching-and-cleanup.md`

Also read `references/retry-branch-and-pivot.md` when multiple approaches, failed branches, comparisons, retries, or pivots become central. For code/debugging work with repeated attempts, use both `references/development-and-troubleshooting.md` and `references/retry-branch-and-pivot.md`.
