# Development and Troubleshooting

Use this reference for long-running:
- implementation
- debugging
- troubleshooting
- refactoring
- migration
- review or audit work

These tasks often stay cleaner when treated as stages with anchors, instead of one uninterrupted thread. If multiple alternative approaches, failed branches, or route comparisons become central, also read `retry-branch-and-pivot.md`.

## Working pattern

1. Create a checkpoint before starting serious work.
2. Add checkpoints before risky edits, new approaches, or major phase changes.
3. Review the timeline when you need to understand the current shape of the work.
4. Rewind after a stable implementation milestone, a failed approach, or a completed troubleshooting phase when there is another phase, attempt, validation step, or task switch that benefits from cleanup. If the completed phase is also the final user-visible deliverable, answer first and wait.

## Typical checkpoint moments

Checkpoint:
- before starting implementation
- before a risky refactor
- before trying an alternative fix
- after a milestone like "root cause confirmed" or "first pass implemented"
- before switching to a side task

Example checkpoint names:
- `parser-fix-start`
- `cache-refactor-attempt-2`
- `migration-plan-ready`
- `incident-root-cause-confirmed`

## Timeline review moments

Run `context_timeline` when:
- multiple attempts now exist
- the task moved from diagnosis to implementation
- you are about to abandon one approach and restart from another anchor
- you are unsure which checkpoint best represents the clean continuation point

## Rewind patterns

### After a failed attempt

Use rewind when an attempt clearly failed and you have a crisp summary of why.

```javascript
context_rewind({
  target: "memory-leak-fix-start",
  message: "WeakRef approach failed because objects were collected too early and cache hit rate collapsed. Reason: abandoning this attempt and returning to a clean anchor. Next step: try object pooling instead.",
  backupCheckpoint: "memory-leak-weakref-raw-history"
});
```

### After a completed phase

Use rewind when a phase is done and you want to continue from a cleaner thread. Do not use this as a reflexive final step after delivering the finished work; use it before validation, the next phase, the next attempt, or the next user task.

```javascript
context_rewind({
  target: "parser-fix-start",
  message: "Parser fix is implemented and the debugging phase is complete. Reason: compacting implementation history before focused validation. Next step: run targeted validation and summarize remaining edge cases.",
  backupCheckpoint: "parser-fix-debug-history"
});
```

## Warning signs

Switch into stronger context-management behavior when:
- you are accumulating many partial theories
- the thread contains multiple fix attempts
- you keep revisiting earlier reasoning
- the next step is clear but the path behind it is getting noisy
