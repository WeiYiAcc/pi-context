# Search, Research, and Reading

Use this reference when the work is mainly driven by large amounts of input material, such as:
- searching
- research
- web search
- browser-driven information gathering
- reading many files, logs, docs, webpages, or web results
- review-heavy reading
- comparison-heavy reading
- audit or inspection across many sources

This reference is for **input-heavy work where the process is much larger than the final conclusion**. It is especially relevant for web search and browser/page reading, where the information density is often low and the raw trail becomes stale quickly. If the main anchor is an explicit plan or todo list, use `planning-and-execution.md` instead.

## Working pattern

1. Create a checkpoint before a large search or reading loop.
2. Search, browse, read, inspect, and follow leads normally.
3. If you lose orientation, review the timeline.
4. Once the investigation yields a stable finding, rewind to a cleaner anchor.
5. Continue with the conclusion, recommendation, or next action instead of carrying the entire raw exploration forward.

Do not stop at "I already made a checkpoint" if the investigation phase is actually complete. The cleanup move for completed research is usually a rewind back to the best earlier anchor.

## When to checkpoint

Checkpoint:
- before the first big search pass
- before opening a browser-heavy or webpage-heavy reading pass
- before diving into a new evidence branch
- after a stable intermediate conclusion
- before changing investigation direction

Example checkpoint names:
- `incident-search-start`
- `vendor-review-evidence-branch`
- `api-timeout-investigation-midpoint`

## When to review timeline

Run `context_timeline` when:
- you have followed several leads
- you are no longer sure what the main anchor is
- the investigation has already produced multiple checkpoints
- you are deciding which path should be compacted

## When to rewind

Rewind after the investigation produces one of these:
- a stable root cause
- a stable comparison result
- a dead-end conclusion
- a shortlist of viable next actions

If you already have one of these, the investigation phase is usually complete enough to compact.

Do not rewind in the middle of a still-open search loop just because the thread feels busy.

## Typical rhythm

```javascript
context_checkpoint({ name: "timeout-investigation-start" });

// ... search logs, read code, compare docs, inspect outputs ...

context_timeline();

context_rewind({
  target: "timeout-investigation-start",
  message: "Found DB connection pool exhaustion as the likely root cause. Reason: investigation phase is complete and ready to compact. Next step: report findings and propose mitigation."
});
```

## Good rewind outcomes

After rewinding, you should be able to continue with:
- the finding
- the recommendation
- the next verification step
- the next narrower investigation

You should not still need the whole raw trail in active context unless the investigation is still ongoing.
