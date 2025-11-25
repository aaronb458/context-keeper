# Project Instructions

## Auto-Load Context

**IMPORTANT:** At the start of every conversation in this project, you MUST:

1. Read `CONTEXT.md` to understand this project
2. Read recent entries from `CHANGELOG.md` to know what's been done

This gives you full project knowledge immediately.

## Project: Context Keeper - Claude Code Plugin

This is a Claude Code plugin that provides automatic project context management. It maintains CONTEXT.md and CHANGELOG.md files that give Claude full project knowledge in every conversation.

**See `CONTEXT.md` for:**
- Plugin architecture and structure
- Available commands
- Hook system design
- Development guidelines

**See `CHANGELOG.md` for:**
- Recent development work
- Version history
- Features added

---

## Automatic Behaviors

### 1. Auto-Detect Decisions
When you hear decision-making language like:
- "Let's go with..."
- "I think we should use..."
- "We decided to..."
- "The better approach is..."

**Automatically ask:** "Should I log this decision to DECISIONS.md?"

If yes, log it with:
- What was decided
- Why (the reasoning)
- Alternatives considered
- Trade-offs accepted

### 2. Auto-Detect Errors
When you help fix an error:

**Automatically ask:** "Error fixed! Should I log this to ERRORS.md for future reference?"

If yes, log it with:
- Error message
- Cause
- Solution
- Files affected

### 3. Auto-Suggest Tests
When new commands, hooks, or significant files are created:

**Automatically ask:** "I notice you created [file]. Would you like me to review it or suggest improvements?"

### 4. Auto-Update on Exit
When the user says goodbye, exits, or ends the conversation:

1. Summarize what was accomplished
2. Ask: "Should I update CHANGELOG.md with this session's work?"
3. If significant decisions were made, ask about logging them
4. Update CONTEXT.md "Current State" if features were completed
5. Remind to update version numbers if new features added

---

## Key Files

- `CONTEXT.md` - Full project knowledge (read at start)
- `CHANGELOG.md` - Change history (update at end)
- `DECISIONS.md` - Architecture decisions (auto-prompt to log)
- `ERRORS.md` - Error solutions (auto-prompt to log)
- `README.md` - Public documentation
- `commands/` - Slash command definitions
- `hooks/` - Automatic hook definitions
- `.claude-plugin/plugin.json` - Plugin manifest (update version!)

---

## Before Ending Conversation

When the user is about to exit:
1. Summarize what was accomplished
2. Offer to update CHANGELOG.md
3. Note any decisions that should be recorded
4. Remind about version bump if new features added
5. Remind to push changes to GitHub
