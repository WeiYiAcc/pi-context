# Context Management Natural Recognition Eval Notes

This set uses realistic user wording and scores recognition by mode-signal evidence instead of requiring immediate tool invocation or reference reads.

## What this set tests

1. **Natural mode recognition**
   - Does the agent recognize that the task shape fits context-management, even if the prompt still lacks enough concrete input to begin real work immediately?

2. **Recognition without over-triggering**
   - Can the agent distinguish these context-management-shaped requests from simple summaries, direct edits, or concept-only discussion?

## How to interpret

A prompt in this set can be a successful positive even if the agent does not immediately call `context_checkpoint` or read a reference file, as long as it clearly signals the right working mode and asks for the next concrete inputs in a way consistent with that mode.

The automated runner treats positive cases as passed when there is evidence of both:
- context-management recognition, either via context tool usage, skill/reference reads, or explicit text about context/checkpoint/timeline/compact-style management
- the expected scenario mode, either via the expected reference read or final text matching that mode's aliases

Negative cases should remain non-operational: no context tools, no skill read, and no reference reads.
