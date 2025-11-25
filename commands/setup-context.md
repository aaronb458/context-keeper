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

## Step 6: Create README Section

Add to the project's README.md (or create CONTEXT_README.md if no README):

```markdown
## Context Tracking

This project uses automatic context tracking to maintain project knowledge across Claude Code conversations.

### Files:
- **CONTEXT.md** - Project overview, patterns, and conventions (auto-loaded)
- **CHANGELOG.md** - Historical record of changes (auto-updated)
- **.claude/context-config.json** - Configuration

### How It Works:
1. **Conversation Start** - CONTEXT.md is automatically loaded, giving Claude full project knowledge
2. **During Work** - Changes are tracked silently in the background
3. **Conversation End** - CHANGELOG.md is automatically updated with your work
4. **After Compaction** - Context persists because it's saved in files, not just conversation history

### Manual Commands:
- `/setup-context` - Regenerate context files
- `/update-context` - Manually update CONTEXT.md
- `/show-context` - Display current context summary
```

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
