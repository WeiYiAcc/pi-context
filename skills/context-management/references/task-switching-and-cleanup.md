# Task Switching and Cleanup

Use this reference when the main problem is not the task domain itself, but a change in thread state, such as:
- the user inserts a temporary side task
- you need to pause one line of work and resume it later
- several active fronts now exist
- the thread is already messy and needs cleanup before continuing
- a finished noisy phase should be summarized and left behind

This reference is for **pause/resume, cleanup, and clean continuation**. It is not for repeated similar items or plan-driven execution.

## Two common variants

### Interruption / task switch
Use when you are actively switching away from one line of work and intend to come back.

### Cleanup and continue
Use when the thread is already stale or messy and you want to compress it now, even though context management is being adopted late.

## Working pattern

1. Inspect timeline if anchor choice is unclear.
2. Before switching away or compacting a noisy path, preserve the best clean anchor.
3. If needed, create a backup checkpoint for the current noisy branch.
4. Handle the side task or cleanup move.
5. Rewind away the stale path when the baton pass is clear.
6. Resume from the paused anchor or continue from the compacted state.

## Useful anchors

Example checkpoint names:
- `primary-task-paused`
- `migration-mainline-paused`
- `release-investigation-paused`
- `cleanup-pre-noise-anchor`

## When to review timeline

Run `context_timeline` when:
- multiple interruptions happened
- the pause lasted many turns
- several side-task branches now exist
- you are unsure which clean anchor should be resumed
- the thread is already messy and you need to find the right pre-noise checkpoint

## When to rewind

Rewind when:
- the interruption created lots of noise
- the side task is done and should not stay active in full
- a stale path is making current reasoning worse
- the useful state is now much smaller than the accumulated process
- you can express the baton pass clearly in a summary

If the interruption was tiny and clean, a rewind may be unnecessary. A checkpoint before switching away is still the key move.

## Common mistakes

Avoid:
- switching away without a pause checkpoint
- returning to the main line while still carrying the side task's full raw path
- trying to clean up without first checking timeline when anchor choice is unclear
- over-resetting and losing still-valid near-term context
