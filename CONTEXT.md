# Project Context: Context Keeper - Claude Code Plugin

> **Last Updated:** 2025-11-24
>
> Automatic project context management that survives conversations and Claude's auto-compaction.

---

## Overview

**Project Name:** Context Keeper
**Purpose:** Claude Code plugin that maintains CONTEXT.md and CHANGELOG.md for persistent project knowledge
**Owner:** Aaron Beadles (@aaronb458)
**Repository:** https://github.com/aaronb458/context-keeper

**Based on:** minimal-claude by @KenKaiii

---

## What This Plugin Does

Solves the problem of Claude forgetting project context between conversations:

1. **CONTEXT.md** - Project knowledge file (auto-loaded)
2. **CHANGELOG.md** - Change history (auto-updated)
3. **Slash commands** - `/setup-context`, `/show-context`, `/update-context`
4. **Hooks** - SessionStart, SessionEnd, PreCompact (auto-triggers)

---

## Plugin Structure

```
context-keeper/
├── .claude-plugin/
│   ├── plugin.json          # Plugin manifest (name, version, author)
│   └── marketplace.json     # Marketplace configuration
├── commands/
│   ├── setup-context.md     # Initialize context tracking
│   ├── show-context.md      # Display context summary
│   ├── update-context.md    # Manually update context
│   └── setup-claude-md.md   # Generate CLAUDE.md (from minimal-claude)
├── hooks/
│   ├── hooks.json           # Hook configuration
│   ├── session-start.md     # Auto-load on conversation start
│   ├── session-end.md       # Auto-save on conversation end
│   └── pre-compact.md       # Save before auto-compaction
├── CLAUDE.md                # Project instructions (this triggers auto-load)
├── CONTEXT.md               # This file - project knowledge
├── CHANGELOG.md             # Change history
└── README.md                # Public documentation
```

---

## Commands

### `/setup-context`
**Purpose:** Initialize context tracking for any project
**Creates:**
- CONTEXT.md (analyzes project, generates knowledge file)
- CHANGELOG.md (creates with initial entries)
- .claude/context-config.json (configuration)

### `/show-context`
**Purpose:** Display current project context summary
**Shows:**
- Project overview
- Current state
- Recent changes
- Token usage

### `/update-context`
**Purpose:** Manually update CONTEXT.md
**Options:**
- Add decisions/patterns
- Update current state
- Add important files
- Auto-analyze changes

---

## Hooks (Automatic Triggers)

### SessionStart
**Trigger:** When conversation starts
**Action:** Silently loads CONTEXT.md and recent CHANGELOG.md
**Result:** Claude immediately knows project

### SessionEnd
**Trigger:** When conversation ends (user exits)
**Action:** Updates CHANGELOG.md with session work
**Result:** Changes are logged automatically

### PreCompact
**Trigger:** Before Claude's auto-compaction
**Action:** Saves context to files before messages removed
**Result:** Context survives compaction

**Note:** Hooks currently work via CLAUDE.md instructions, not as executable scripts.

---

## How Auto-Loading Actually Works

Since Claude Code plugin hooks are prompt-based (not executable scripts), the automatic loading works through **CLAUDE.md**:

1. User runs `claude` in project folder
2. Claude Code reads CLAUDE.md automatically
3. CLAUDE.md contains instructions to read CONTEXT.md
4. Claude reads CONTEXT.md and CHANGELOG.md
5. Claude now has full project knowledge

**This is the production solution** - CLAUDE.md is the trigger mechanism.

---

## Installation

### For Users:
```bash
# Add marketplace
claude plugin marketplace add aaronb458/context-keeper

# Install plugin
claude plugin install context-keeper@context-keeper-marketplace

# In any project:
/setup-context
```

### For Development:
```bash
cd /Users/aaronbeadles/projects/context-keeper
claude
# Make changes, test locally
```

---

## Version History

- **1.0.0** - Initial release with commands
- **1.1.0** - Added hooks for automation

---

## Current State

### ✅ Completed
- Plugin structure and manifest
- `/setup-context` command
- `/show-context` command
- `/update-context` command
- Hook definitions (SessionStart, SessionEnd, PreCompact)
- README documentation
- GitHub repository published
- Plugin installable via marketplace

### 🚧 In Progress
- Testing automatic features
- Refining hook behavior

### 📋 Upcoming (Planned Plugins)
- **decision-tracker** - Architecture Decision Records
- **error-memory** - Remember past errors & solutions
- **test-suggester** - Auto-suggest missing tests

---

## Development Guidelines

### Adding Commands:
1. Create `commands/command-name.md`
2. Add frontmatter with `name` and `description`
3. Write the command prompt/instructions
4. Test with `/command-name`

### Updating Version:
1. Edit `.claude-plugin/plugin.json` - update version
2. Edit `.claude-plugin/marketplace.json` - update version
3. Commit and push to GitHub
4. Users pull updates automatically

### Token Efficiency:
- Keep CONTEXT.md under 3,000 tokens
- Load only 30 days of CHANGELOG
- Total auto-load should be ~3,500 tokens max

---

## Key Decisions

### 2025-11-24: CLAUDE.md as Trigger Mechanism
**Decision:** Use CLAUDE.md to trigger context loading instead of executable hooks
**Reason:** Claude Code plugin hooks are prompt-based, not shell scripts
**Trade-off:** Slightly less "magical" but works reliably

### 2025-11-24: Forked from minimal-claude
**Decision:** Fork minimal-claude as base instead of starting from scratch
**Reason:** Proven plugin structure, existing command patterns
**Trade-off:** Some cleanup needed, but faster development

---

## Notes for Claude

### When Working on This Project:
1. This is a Claude Code plugin - test changes by running commands
2. Update version numbers in BOTH plugin.json and marketplace.json
3. Push to GitHub for changes to be available to users
4. Keep documentation in sync with functionality

### Quality Standards:
- Commands should have clear step-by-step instructions
- README should be comprehensive for new users
- Token efficiency is critical - keep context files lean

---

*This context file is maintained by the Context Keeper plugin itself.*
*Last auto-update: 2025-11-24*
