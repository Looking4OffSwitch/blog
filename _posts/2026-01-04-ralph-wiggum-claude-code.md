---
layout: single
title: "A Claude Code plugin for iterative, self-referential AI development loops."
date: 2026-01-04
categories: claude-code
---

# ralph-wiggum

A Claude Code plugin for iterative, self-referential AI development loops.

## Overview

`ralph-wiggum` enables Claude Code to work on long-running tasks autonomously by implementing a stop-hook pattern that intercepts session exits and re-prompts Claude with the original task. This allows you to define a list of tasks and have Claude work through them for hours without manual intervention.

**Use cases:**

- Process all open pull requests in a repository
- Work through a multi-step project setup checklist
- Batch refactoring across a codebase
- Any workflow with N discrete tasks that can be tracked via a TODO file

## Installation

1. Launch Claude Code
2. Run `/plugin`
3. Select **Discover** → search for `ralph-wiggum`
4. Install in your desired scope (project or global)
5. Restart Claude Code

## Usage

### 1. Create a TODO file

Create a `TODO.md` (or similar) with checkbox-style tasks:

```markdown
# Project Setup

## Tasks
- [ ] 1. Initialize project with npm
- [ ] 2. Configure ESLint and Prettier
- [ ] 3. Set up folder structure
- [ ] 4. Create TypeScript types
- [ ] 5. Create API routes
- [ ] 6. Build frontend components
- [ ] 7. **HARD STOP** - Checkpoint: verify components render correctly
- [ ] 8. Add tests
- [ ] 9. Final build and deploy prep

## Success Criteria
- All tests pass
- Build completes without errors
```

The optional `**HARD STOP**` marker pauses iteration for manual verification before continuing.

### 2. Start the loop

```
/ralph-wiggum:ralph-loop PROMPT [--max-iterations N] [--completion-promise TEXT]
```

**Parameters:**

| Parameter | Description | Default |
|-----------|-------------|---------|
| `PROMPT` | The instruction for Claude to follow | Required |
| `--max-iterations` | Maximum number of iteration cycles | None |
| `--completion-promise` | Text that signals successful completion | None |

**Example:**

```
/ralph-wiggum:ralph-loop "Go through TODO.md step-by-step and check off every step once complete. When you encounter a task marked HARD STOP, use AskUserQuestion to get confirmation before proceeding. If blocked, output <promise>BLOCKED</promise> with an explanation." --completion-promise "DONE" --max-iterations 50
```

### 3. Troubleshooting permissions

If you encounter `Error: Bash command permission check failed for pattern`, add a permission rule to your Claude Code settings (`.claude/settings.json` or via `/config`):

```json
{
  "permissions": {
    "allow": [
      "Bash(**/ralph-wiggum/**)"
    ]
  }
}
```

## How It Works

### The Iteration Loop

```
┌─────────────────────────────────────────────────────────────┐
│  1. Claude receives your prompt                             │
│  2. Claude works (tool calls, file edits, task completion)  │
│  3. Claude decides it's "done" and attempts to exit         │
│  4. Stop hook intercepts → increments iteration counter     │
│  5. Original prompt is re-fed to Claude                     │
│  6. Claude sees modified files and continues working        │
│  7. Repeat until max-iterations or completion-promise found │
└─────────────────────────────────────────────────────────────┘
```

### What counts as one iteration?

**One iteration = one complete Claude session cycle** (when Claude attempts to exit).

It is *not* per tool call or per TODO item. If Claude completes 10 tasks in a single session before attempting to exit, that counts as 1 iteration.

## Why This Pattern Matters

### The Problem: Claude's Session-Oriented Design

Claude Code is designed around discrete prompt-response sessions. When you ask Claude to complete a task, it works until it believes the task is done, then exits. For a single, well-defined task, this works perfectly.

But many real-world workflows involve **N sequential tasks** where N can be large:

- "Implement these 47 feature requests"
- "Refactor all components to use the new API"
- "Process every file in this directory"

Without `ralph-wiggum`, you'd need to manually re-prompt Claude after each exit, watching the terminal and typing variations of "continue" or "do the next one" dozens of times.

### The Solution: Stop-Hook Interception

`ralph-wiggum` solves this by intercepting Claude's exit signal and automatically re-injecting the original prompt. Claude sees the current state of the filesystem (including any TODO checkboxes it marked complete) and picks up where it left off.

This transforms Claude from a **single-task executor** into an **autonomous task runner** that can work through arbitrarily long task lists.

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **Unattended execution** | Walk away while Claude works through hours of tasks |
| **State persistence** | File modifications persist between iterations; Claude sees its own progress |
| **Controlled checkpoints** | Use `HARD STOP` markers to pause for human verification |
| **Bounded execution** | `--max-iterations` prevents runaway loops |
| **Completion detection** | `--completion-promise` allows Claude to signal when truly done |

### When to Use This

✅ **Good fit:**
- Multi-step project scaffolding
- Batch file processing
- Working through a backlog of issues/PRs
- Any task list where steps are independent or linearly dependent

⚠️ **Consider carefully:**
- Tasks requiring frequent human judgment
- Workflows where errors compound (consider more `HARD STOP` checkpoints)
- Tasks with external dependencies that may timeout or rate-limit

## Documentation

Full plugin source and additional documentation:  
https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum

## License

See the [Claude Code repository](https://github.com/anthropics/claude-code) for license information.
