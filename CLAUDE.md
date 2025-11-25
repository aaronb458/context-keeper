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

## Before Ending Conversation

When the user is about to exit or says goodbye:
1. Summarize what was accomplished
2. Suggest updating CHANGELOG.md with the work done
3. Note any decisions that should be recorded

## Key Files

- `CONTEXT.md` - Full project knowledge
- `CHANGELOG.md` - Change history
- `README.md` - Public documentation
- `commands/` - Slash command definitions
- `hooks/` - Automatic hook definitions
- `.claude-plugin/plugin.json` - Plugin manifest
