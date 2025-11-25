---
name: session-start
description: Auto-load CONTEXT.md at the start of every conversation
hook: SessionStart
---

## Auto-Load Project Context

This hook automatically loads CONTEXT.md at the start of every conversation, giving you immediate project knowledge.

### Step 1: Check for CONTEXT.md

```bash
ls CONTEXT.md 2>/dev/null
```

If CONTEXT.md doesn't exist:
- **DO NOTHING** - Don't interrupt the user's workflow
- User can run `/setup-context` manually if they want context tracking
- Exit silently

If CONTEXT.md exists:
- Continue to Step 2

### Step 2: Check Configuration

Read `.claude/context-config.json` if it exists:

```bash
cat .claude/context-config.json 2>/dev/null
```

Check if `auto_load_context` is enabled.

If config doesn't exist or `auto_load_context` is false:
- Exit silently (user disabled auto-loading)

If `auto_load_context` is true:
- Continue to Step 3

### Step 3: Read CONTEXT.md

Read the full CONTEXT.md file:

```bash
cat CONTEXT.md
```

### Step 4: Load Recent CHANGELOG Entries

If CHANGELOG.md exists, read recent entries (last 30 days by default):

```bash
cat CHANGELOG.md 2>/dev/null
```

Extract entries from the last 30 days (or `changelog_days` from config).

### Step 5: Silently Inject Context

**IMPORTANT:** Do NOT display anything to the user. Do NOT say "Loaded CONTEXT.md" or any message.

The context is now loaded in your system memory. You have full knowledge of:
- Project overview and purpose
- Tech stack and architecture
- Patterns and conventions
- Current state (completed/in-progress/upcoming)
- Recent changes from CHANGELOG
- Important files and their purposes
- Notes and guidelines for working on this project

### Step 6: Be Ready

When the user starts working, you already know:
- What this project is
- How it's structured
- What patterns to follow
- What's been done recently
- What's currently in progress

**No need to ask clarifying questions about the project basics.**

If the user asks "What is this project?" or "What have we been working on?", you can answer immediately from the loaded context.

### Notes:

- This runs **automatically** every time a conversation starts
- It's **silent** - user doesn't see any loading message
- It's **fast** - just reads two markdown files
- It's **token-efficient** - only ~3,000-4,000 tokens
- It **persists across compactions** - because it reloads from files

Done!
