# Planning and Execution

Use this reference when the work is organized around an explicit execution frame, such as:
- a formal implementation or migration plan
- a roadmap with multiple milestones
- a lightweight todo list or checklist
- staged execution that will be revisited, updated, or reordered across many turns

The defining feature here is not just that the work has phases. It is that execution repeatedly returns to an explicit plan, roadmap, or todo baseline.

## Two common variants

### Formal plan
Examples:
- a plan produced by a planning step or planning agent
- a migration plan with several services or phases
- a rollout plan with checkpoints and validation steps

### Lightweight todo / checklist
Examples:
- a short task list for a refactor
- a checklist for a release or audit
- a small execution outline that will be worked through item by item

Both variants use the same context-management rhythm.

## Working pattern

1. Create or confirm the plan / todo list.
2. Create a checkpoint for the clean plan-ready state.
3. Execute one subtask or phase.
4. If that subtask becomes noisy, let it get noisy locally.
5. Once the subtask produces a stable takeaway, rewind back to the plan-ready or phase-start anchor.
6. Continue with the next subtask from the clean anchor.
7. If the plan changes materially, checkpoint the updated plan state again.

## Useful anchors

Example checkpoint names:
- `auth-migration-plan-ready`
- `release-rollout-plan-ready`
- `parser-refactor-todo-baseline`
- `cleanup-phase-2-start`
- `vendor-audit-milestone-1`

## When to review timeline

Run `context_timeline` when:
- several subtasks have already been executed
- the plan has changed more than once
- multiple branches now exist under the same plan
- you are unsure whether to rewind to the overall plan anchor or the current phase-start anchor

## When to rewind

Rewind when:
- a subtask is complete and its raw execution path is no longer worth keeping active
- a phase finished and the next phase should start from a cleaner state
- the plan remains valid but the current execution segment has become noisy

Do not rewind just because a todo list exists. Rewind when a specific execution segment has already served its purpose and can be compacted.

## Replan

If the plan itself changes materially:
1. finish or abandon the current noisy segment
2. summarize the change in direction
3. checkpoint the new plan-ready state
4. continue from the updated plan anchor

If the plan change is driven by failed branches or strategy shifts, also read `retry-branch-and-pivot.md`.

## Common mistakes

Avoid:
- keeping every finished subtask's raw reasoning active
- rewinding too far back when the current plan-ready anchor is still valid
- failing to checkpoint the updated plan after a major replan
- confusing repeated-item work with plan-driven work when the subtasks are actually different in nature
