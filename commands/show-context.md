---
name: show-context
description: Display current project context summary
---

Display a summary of the current project context from CONTEXT.md and recent CHANGELOG entries.

## Step 1: Check for Context Files

```bash
ls CONTEXT.md CHANGELOG.md 2>/dev/null
```

If files don't exist:
- Tell user: "Context tracking not set up. Run `/setup-context` to initialize."
- Exit

## Step 2: Read and Display Context Summary

### From CONTEXT.md:
Extract and display:
- Project name and purpose
- Tech stack
- Current state (in-progress items)
- Recent decisions/patterns (last 3)

### From CHANGELOG.md:
Extract and display:
- Last 3 changelog entries
- Any unreleased/in-progress items

## Step 3: Show Token Usage

Calculate and display:
- CONTEXT.md size: ~[X] tokens
- CHANGELOG.md size (last 30 days): ~[Y] tokens
- Total auto-load: ~[Z] tokens

## Step 4: Show Auto-Tracking Status

From `.claude/context-config.json`:
- Auto-load enabled: [Yes/No]
- Auto-update enabled: [Yes/No]
- Session tracking: [Yes/No]

## Display Format:

```
📋 Project Context Summary

## Project: [Name]
**Purpose:** [One-line description]
**Tech:** [Stack]

## Current Work:
🚧 [In-progress item 1]
🚧 [In-progress item 2]

## Recent Changes:
✅ [Recent change 1] ([Date])
✅ [Recent change 2] ([Date])

## Context Stats:
- CONTEXT.md: ~[X] tokens ([Y] lines)
- CHANGELOG.md: ~[X] tokens (last 30 days)
- Total auto-load: ~[X] tokens

## Auto-Tracking Status:
✅ Context auto-loads on conversation start
✅ Changes auto-tracked during work
✅ Changelog auto-updates on exit
✅ Pre-compaction saves enabled

Use `/update-context` to manually update project context.
```

Done!
