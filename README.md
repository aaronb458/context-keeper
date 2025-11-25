# Context Keeper - Claude Code Plugin

**Automatic project context management that survives conversations and Claude's auto-compaction.**

Never lose project context again. Context Keeper automatically maintains `CONTEXT.md` and `CHANGELOG.md` files that give Claude full project knowledge in every conversation - even after auto-compaction.

---

## The Problem

When working with Claude Code:
- ❌ Claude forgets project context between conversations
- ❌ Auto-compaction removes conversation history
- ❌ You have to re-explain your project every time
- ❌ MCP servers consume 30k+ tokens just for tool definitions
- ❌ No permanent record of decisions and patterns

## The Solution

Context Keeper creates and maintains two key files:

###  **CONTEXT.md** - Your Project's Brain
- Auto-loads at the start of every conversation
- Contains project overview, patterns, and conventions
- ~2,500-3,000 tokens (vs. 31k for MCP servers!)
- Claude immediately knows your project

### **CHANGELOG.md** - Your Project's Memory
- Auto-updated when you finish work
- Tracks features, decisions, and changes
- Survives auto-compaction
- Historical record of your project

---

## Quick Start

### 1. Install the Plugin

```bash
# Add the marketplace
claude plugin marketplace add aaronb458/context-keeper

# Install the plugin
claude plugin install context-keeper@aaronb458
```

### 2. Initialize Context Tracking

```bash
cd your-project
claude

# In Claude Code:
/setup-context
```

That's it! Context tracking is now active.

### 3. See It in Action

Exit and restart Claude in the same project:

```bash
exit
claude
```

Ask Claude: "What is this project?"

Claude will know immediately - no re-explaining needed!

---

## How It Works

### Automatic Flow:

```
1. You enter project folder → claude

2. CONTEXT.md auto-loads
   ✅ Claude knows your project instantly
   ✅ Understands patterns and conventions
   ✅ Aware of current state

3. You work together
   🔍 Changes tracked silently in background
   📝 Decisions detected automatically

4. You exit conversation
   ✅ CHANGELOG.md auto-updates with your work
   ✅ CONTEXT.md updated if patterns changed
   ✅ Session logged for reference

5. Next conversation
   ✅ Full context restored from files
   ✅ Even after auto-compaction!
```

---

## Commands

### `/setup-context`
Initialize context tracking for your project.

**What it creates:**
- `CONTEXT.md` - Project knowledge file
- `CHANGELOG.md` - Change history
- `.claude/context-config.json` - Configuration

**Usage:**
```bash
/setup-context
```

### `/update-context`
Manually update CONTEXT.md with new information.

**Options:**
1. Add a new decision or pattern
2. Update current state
3. Add new important files
4. Update project overview
5. Add notes for Claude
6. Auto-analyze recent changes

**Usage:**
```bash
/update-context
```

### `/show-context`
Display current project context summary.

**Shows:**
- Project overview and tech stack
- Current work in progress
- Recent changes
- Token usage stats
- Auto-tracking status

**Usage:**
```bash
/show-context
```

---

## What Gets Tracked

### CONTEXT.md Contains:

**✅ Project Overview**
- Name, purpose, tech stack
- Project structure
- Key architectural patterns

**✅ Patterns & Conventions**
- Naming conventions
- Coding standards
- Design patterns used

**✅ Current State**
- Completed features
- Work in progress
- Upcoming tasks

**✅ Important Files**
- Key source files
- Configuration files
- Documentation

**✅ Notes for Claude**
- Quality standards
- Workflow preferences
- Things to remember

### CHANGELOG.md Contains:

**✅ Historical Record**
- Features added
- Changes made
- Bugs fixed
- Decisions documented

**✅ Dated Entries**
- Recent changes (last 30 days auto-load)
- Full history (archived after 90 days)

---

## Token Usage

**Context Keeper:** ~3,000-4,000 tokens total
- CONTEXT.md: ~2,500-3,000 tokens
- CHANGELOG.md (30 days): ~500-1,000 tokens

**vs. MCP Servers:** ~31,000+ tokens
- Just for tool definitions!
- No actual project knowledge

**You save:** ~27,000 tokens = more room for your code!

---

## Configuration

Edit `.claude/context-config.json` to customize:

```json
{
  "auto_mode": {
    "enabled": true,              // Auto-load/update
    "auto_load_context": true,    // Load CONTEXT.md on start
    "auto_log_changes": true,     // Track changes during work
    "auto_update_context": true,  // Update CONTEXT.md on exit
    "pre_compact_save": true      // Save before compaction
  },

  "loading": {
    "changelog_days": 30,         // Days of changelog to load
    "max_context_tokens": 3000,   // Token limit for CONTEXT.md
    "summary_mode": false         // Use condensed version
  },

  "triggers": {
    "min_files_changed": 1,       // Min changes to log
    "detect_decisions": true,     // Auto-detect decisions
    "detect_patterns": true       // Auto-detect patterns
  }
}
```

---

## Examples

### Example CONTEXT.md

```markdown
# Project Context: CustomerHub SaaS

> Last Updated: 2025-11-24

## Overview
**Purpose:** B2B customer relationship management for small businesses
**Tech Stack:** Next.js 14, TypeScript, PostgreSQL, Prisma, Tailwind CSS

## Architecture
- App Router with server components
- JWT authentication with refresh tokens
- RESTful API under /api/
- PostgreSQL with Prisma ORM

## Key Patterns
**Authentication:** JWT tokens in httpOnly cookies
**Data Fetching:** Server components for DB queries
**API Format:** { success: boolean, data?: any, error?: string }

## Current State
✅ User authentication complete
🚧 Customer management in progress
📋 Deal pipeline upcoming

## Notes for Claude
- Always run type checks after TypeScript edits
- Follow API response format in all endpoints
- Update CONTEXT.md for architectural decisions
```

### Example CHANGELOG.md

```markdown
# Changelog

## 2025-11-24

### Added
- User authentication system with JWT tokens
  - Login/logout endpoints
  - Protected route middleware
  - Files: `lib/auth.ts`, `middleware/auth.ts`

### Changed
- Refactored API error handling
  - Centralized error middleware
  - File: `lib/errors.ts`

### Decisions
- **JWT vs Sessions:** Chose JWT for API-first architecture
  - Better for mobile app integration
  - Trade-off: More complex token refresh logic
```

---

## Benefits

### For You:
- ✅ Never re-explain your project
- ✅ Permanent record of decisions
- ✅ Easy onboarding for team members
- ✅ Track project evolution over time
- ✅ Context survives compaction

### For Claude:
- ✅ Always has project context
- ✅ Follows established patterns
- ✅ Knows what's been done
- ✅ Can reference past decisions
- ✅ Understands your preferences

---

## Advanced Features

### Manual Context Loading
```markdown
<!-- In your prompt -->
Please review the authentication code.
Context from CONTEXT.md will be automatically loaded.
```

### Session Logs
Track detailed work in `.claude/session-logs/`:
```json
{
  "session_id": "2025-11-24-001",
  "start": "2025-11-24T14:30:00Z",
  "end": "2025-11-24T15:45:00Z",
  "files_changed": ["lib/auth.ts", "middleware.ts"],
  "summary": "Implemented JWT authentication system"
}
```

### Auto-Pruning
Old changelog entries (>90 days) automatically move to `CHANGELOG_ARCHIVE.md`:
- Keeps main CHANGELOG lean
- Archive searchable when needed
- No data loss

---

## Installation

### From GitHub

```bash
# Add marketplace
claude plugin marketplace add aaronb458/context-keeper

# Install plugin
claude plugin install context-keeper@aaronb458
```

### Local Testing

```bash
# Clone the repo
git clone https://github.com/aaronb458/context-keeper.git

# Add local marketplace
claude plugin marketplace add /path/to/context-keeper

# Install
claude plugin install context-keeper
```

---

## Updating the Plugin

When new versions are released:

```bash
# Check for updates
claude plugin list

# Update if available
claude plugin update context-keeper
```

---

## Troubleshooting

### CONTEXT.md not loading?
1. Check `.claude/context-config.json` exists
2. Verify `auto_load_context: true`
3. Restart Claude Code

### Token limit warnings?
1. Run `/show-context` to check token count
2. Condense older sections in CONTEXT.md
3. Enable `summary_mode: true` in config
4. Move detailed info to separate docs

### Changelog not updating?
1. Check `auto_log_changes: true` in config
2. Verify you made significant changes (min_files_changed)
3. Check `.claude/session-logs/` for session data

---

## Contributing

Found a bug? Have a feature request?

**GitHub:** https://github.com/aaronb458/context-keeper
**Issues:** https://github.com/aaronb458/context-keeper/issues

---

## License

MIT License - See LICENSE file for details

---

## Credits

Built with ❤️ by [@aaronb458](https://github.com/aaronb458)

Based on the [minimal-claude](https://github.com/KenKaiii/minimal-claude) plugin framework by [@KenKaiii](https://github.com/KenKaiii)

---

**Never lose project context again. Install Context Keeper today!**

```bash
claude plugin install context-keeper@aaronb458
```
