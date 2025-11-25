---
name: setup-context
description: Initialize automatic context tracking with CONTEXT.md and CHANGELOG.md
---

Initialize automatic project context tracking that persists across conversations and survives Claude's auto-compaction.

## What This Does

Creates and configures automatic context management for your project:

1. **CONTEXT.md** - Master project knowledge file (auto-loaded in every conversation)
2. **CHANGELOG.md** - Automatic change tracking (updated when you finish work)
3. **.claude/context-config.json** - Configuration for auto-tracking

## Step 1: Check Existing Files

Check if context tracking is already set up:

```bash
ls CONTEXT.md CHANGELOG.md .claude/context-config.json 2>/dev/null
```

If files exist:
- Ask user: "Context tracking is already set up. Would you like to regenerate/update the files?"
- If no → Exit
- If yes → Continue to regenerate

## Step 2: Analyze Project

Analyze the project to build context:

### Detect Project Type:
- Check for `package.json` → JavaScript/TypeScript/Node.js
- Check for `requirements.txt` or `pyproject.toml` → Python
- Check for `Cargo.toml` → Rust
- Check for `go.mod` → Go
- Check for `.md` files → Documentation project
- Check for `.docx` or templates → Document generation project

### Gather Project Information:
1. **Project name** - From directory name or package.json
2. **Purpose** - From README.md or infer from files
3. **Tech stack** - Languages, frameworks, tools
4. **File structure** - Key directories and their purposes
5. **Existing documentation** - README, docs folder, guide files
6. **Recent work** - Check git log for recent commits (if git repo)

## Step 3: Generate CONTEXT.md

Create a comprehensive CONTEXT.md file with these sections:

```markdown
# Project Context: [Project Name]

> **Last Updated:** [Current Date]
>
> This file contains the essential knowledge about this project.

---

## Overview

**Project Name:** [Name]
**Purpose:** [What this project does]
**Tech Stack:** [Languages, frameworks, tools]

---

## Project Structure

[Directory tree with descriptions]

```
src/
  ├── [folder]/  # [Description]
  ├── [folder]/  # [Description]
```

---

## Key Patterns & Conventions

[Coding patterns, naming conventions, architectural decisions]

---

## Current State

### ✅ Completed
- [Feature 1]
- [Feature 2]

### 🚧 In Progress
- [Current work]

### 📋 Upcoming
- [Planned features]

---

## Important Files

### [Category]:
- `[file]` - [Description]
- `[file]` - [Description]

---

## Notes for Claude

### When Working on This Project:
1. [Guideline 1]
2. [Guideline 2]
3. [Guideline 3]

---

*This context file is maintained automatically.*
*Last auto-update: [Date]*
```

## Step 4: Generate CHANGELOG.md

Create a CHANGELOG.md file with historical context:

```markdown
# Changelog: [Project Name]

All notable changes to this project will be documented in this file.

---

## [Unreleased]

### In Progress
- Automatic context tracking system

---

## [Current Date]

### Added
- **CONTEXT.md** - Project context file
  - Complete overview of project
  - Architecture and patterns
  - File: `CONTEXT.md`

- **CHANGELOG.md** - Project changelog
  - Historical record of changes
  - File: `CHANGELOG.md`

- **.claude/context-config.json** - Auto-tracking configuration
  - Enables automatic context loading
  - File: `.claude/context-config.json`

### Context
- Setting up automatic context management
- Enables Claude to maintain full project knowledge across conversations
- Survives auto-compaction

---

[If git history exists, extract recent commits and add entries]

---

*This changelog is maintained automatically.*
*Last auto-update: [Date]*
```

## Step 5: Generate Configuration

Create `.claude/context-config.json`:

```json
{
  "project_name": "[Project Name]",
  "version": "1.0.0",
  "created": "[Current Date]",

  "auto_mode": {
    "enabled": true,
    "auto_load_context": true,
    "auto_log_changes": true,
    "auto_update_context": true,
    "pre_compact_save": true
  },

  "triggers": {
    "min_files_changed": 1,
    "min_conversation_length": 10,
    "detect_decisions": true,
    "detect_patterns": true
  },

  "loading": {
    "changelog_days": 30,
    "max_context_tokens": 3000,
    "summary_mode": false
  },

  "context_sections": [
    "overview",
    "structure",
    "patterns",
    "current_state"
  ],

  "ignore_patterns": [
    "*.DS_Store",
    ".git",
    "node_modules",
    "*.tmp"
  ],

  "session_tracking": {
    "enabled": true,
    "log_directory": ".claude/session-logs",
    "max_session_logs": 30
  }
}
```

## Step 6: Generate CLAUDE.md (CRITICAL - This enables all automation!)

Create `CLAUDE.md` with automatic behavior instructions:

```markdown
# Project Instructions

## Auto-Load Context

**IMPORTANT:** At the start of every conversation in this project, you MUST:

1. Read `CONTEXT.md` to understand this project
2. Read recent entries from `CHANGELOG.md` to know what's been done

This gives you full project knowledge immediately.

## Project: [Project Name]

[Brief 1-2 sentence description]

**See `CONTEXT.md` for:**
- Complete project overview
- Architecture and patterns
- Conventions and standards
- Current state

**See `CHANGELOG.md` for:**
- Recent work history
- Changes made
- Decisions documented

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
When new functions or components are created:

**Automatically ask:** "I notice you created [file]. Would you like me to suggest tests for it?"

### 4. Auto-Update on Exit
When the user says goodbye, exits, or ends the conversation:

1. Summarize what was accomplished
2. Ask: "Should I update CHANGELOG.md with this session's work?"
3. If significant decisions were made, ask about logging them
4. Update CONTEXT.md "Current State" if features were completed

---

## Key Files

- `CONTEXT.md` - Full project knowledge (read at start)
- `CHANGELOG.md` - Change history (update at end)
- `DECISIONS.md` - Architecture decisions (auto-prompt to log)
- `ERRORS.md` - Error solutions (auto-prompt to log)

---

## Before Ending Conversation

When the user is about to exit:
1. Summarize what was accomplished
2. Offer to update CHANGELOG.md
3. Note any decisions that should be recorded
4. Suggest tests for new code if applicable
```

This CLAUDE.md file is what makes everything automatic - Claude reads it at conversation start and follows the instructions throughout.

## Step 7: Summary

Display to user:

```
✅ Context tracking initialized!

Created:
- CONTEXT.md (~[X] lines, ~[Y] tokens)
- CHANGELOG.md (~[X] lines)
- .claude/context-config.json

How it works:
1. Every conversation: CONTEXT.md auto-loads (Claude knows your project immediately)
2. As you work: Changes are tracked automatically
3. When you exit: CHANGELOG.md auto-updates with session work
4. After compaction: Context persists in files (nothing lost!)

Token usage: ~[X] tokens (much better than 31k from MCP servers!)

Next conversation in this folder:
  cd [project-path]
  claude

Claude will automatically know:
  ✅ Project purpose and structure
  ✅ Patterns and conventions
  ✅ Recent work and decisions
  ✅ Current state and next steps

No re-explaining needed!
```

## Important Notes:

1. **Token Efficiency**:
   - Keep CONTEXT.md under 3,000 tokens (typically 400-500 lines)
   - Load only last 30 days of CHANGELOG by default
   - Total auto-load: ~3,000-4,000 tokens

2. **What to Include in CONTEXT.md**:
   - Project overview and purpose
   - Key architectural patterns
   - Naming conventions
   - File structure
   - Important decisions
   - Quality standards
   - Notes for Claude

3. **What NOT to Include**:
   - Full file contents
   - Detailed API documentation
   - Complete code examples
   - Verbose explanations

4. **Update Strategy**:
   - Auto-update on significant changes
   - Manual update via `/update-context` when needed
   - Keep concise and actionable

Done!
