# PROJECT KNOWLEDGE BASE

**Generated:** 2026-03-04

## OVERVIEW
Project: **pi-context**
Stack: Node.js, TypeScript (ES2022 / Node16), `@earendil-works/pi-coding-agent` API, `@earendil-works/pi-ai` Type/type schemas.

Description: An Agentic Context Management extension for the `pi` coding agent. It allows AI agents to proactively structure, inspect, and compact their context (history) using explicit checkpoints, timeline inspection, and rewind-based cleanup.

## STRUCTURE
*   `src/`: TypeScript source code for the extension.
    *   `index.ts`: Tool and command registrations (`acm`, `context_checkpoint`, `context_timeline`, `context_rewind`).
    *   `context.ts`: CLI command registrations (e.g., `/context` for TUI visualization).
    *   `utils.ts`: Shared utility functions and type definitions.
*   `skills/`: Pi skills documentation, containing `context-management/SKILL.md` which instructs the LLM on how to use the context tools.
*   `test/`: Markdown test scenarios.

## COMMANDS
| Action | Command |
|--------|---------|
| Install| `npm install` |
| Typecheck | `npm run typecheck` |
| Test   | See `test/test.md` |
| Run    | The extension is loaded natively by `pi`. In a `pi` environment, run `pi -e ./src/index.ts -e ./src/context.ts --skill ./skills` to test locally. |

## CODING STANDARDS
*   **Language**: TypeScript with ESM (`"type": "module"`).
*   **Style**: Standard TypeScript. Interfaces and functions are exported as ES modules. Tool parameter schemas use `Type` from `@earendil-works/pi-ai`.
*   **Rules**: Strict mode is enabled (`"strict": true`). Missing types from the main SDK entry point are defined locally (e.g., `SessionTreeNode` in `utils.ts`).

## WHERE TO LOOK
*   **Source**: `src/`
*   **Tests**: `test/`
*   **Skill Docs**: `skills/context-management/SKILL.md` (Crucial for understanding the agent workflow, checkpointing strategy, timeline review, and rewind-based cleanup).

## NOTES
*   **Architecture**: `pi-context` hooks directly into the `SessionManager` from the `pi-coding-agent` SDK, leveraging its underlying tree structure to implement lossless time travel and conversation-history compaction without deleting nodes from disk.
*   **Runtime**: The package is source-first. Pi loads the TypeScript extension files declared in `pi.extensions` (`src/index.ts`, `src/context.ts`); no `dist/` build artifact is required for publishing.
*   **Test Script Issues**: `test/test.md` outlines the testing steps.
