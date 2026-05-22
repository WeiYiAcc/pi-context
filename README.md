# Pi Context: Agentic Context Management for Pi

An Agentic Context Management tool that helps AI agents proactively manage checkpoints, timeline inspection, and rewind-based context cleanup.

Inspired by kimi-cli d-mail, it brings lossless time travel to Pi's session tree.

For more on the design philosophy, see the [blog post](https://blog.xlab.app/p/51d26495/) ([中文版本](https://blog.xlab.app/p/6a966aeb/)).

## Naming migration note

Earlier versions used more Git-like names such as `context_tag`, `context_log`, and `context_checkout`.

Current versions intentionally use conversation-native names instead:
- `context_checkpoint`
- `context_timeline`
- `context_rewind`

These tools manage **conversation history**, not repository state. They should not be treated as Git commands or as replacements for real `git tag`, `git log`, or `git checkout`.

## Installation

```bash
pi install npm:pi-context
```

## Usage

### For Humans

Run the following command to enable ACM (**A**gentic **C**ontext **M**anagement) for the current session.

```bash
/acm
```

Open a visual dashboard to inspect context-window usage and token distribution (similar to `claude code /context`).

```bash
/context
```

![](img/context.png)

### For Agents

This extension adds the `context-management` skill, which includes three core tools:

1. **🔖 Structure (`context_checkpoint`)**
   Create named checkpoints to organize conversation history.

2. **📊 Monitor (`context_timeline`)**
   Visualize conversation history, inspect token usage, and see where you are in the task tree.

3. **⏪ Compress (`context_rewind`)**
   Return to an earlier checkpoint or anchor with a carryover summary so completed noisy work can be compacted into a cleaner continuation.
