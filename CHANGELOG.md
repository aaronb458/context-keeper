# Changelog: Context Keeper Plugin

All notable changes to this project will be documented in this file.

---

## [Unreleased]

### In Progress
- Testing automatic context loading

---

## 1.2.0 - 2025-11-24

### Added
- **`/log-decision`** - Log architecture decisions (ADR format)
  - Records what, why, alternatives, trade-offs
  - Stores in DECISIONS.md
  - Also logs to CHANGELOG.md
  - File: `commands/log-decision.md`

- **`/show-decisions`** - Display logged decisions
  - Summary view of all decisions
  - Filter by keyword
  - View full decision details
  - File: `commands/show-decisions.md`

- **`/log-error`** - Log errors and solutions
  - Tracks error message, cause, solution
  - Categorizes errors automatically
  - Searchable by keywords
  - File: `commands/log-error.md`

- **`/search-errors`** - Search error log for solutions
  - Fuzzy matching on error messages
  - Auto-suggests when errors detected
  - Never solve same problem twice!
  - File: `commands/search-errors.md`

- **`/suggest-tests`** - Analyze code for missing tests
  - Detects untested files
  - Prioritizes by criticality
  - Generates test suggestions
  - Can create test files
  - File: `commands/suggest-tests.md`

### Summary
- **Decision Tracking**: Know WHY you made choices
- **Error Memory**: Never debug the same thing twice
- **Test Suggestions**: Ensure quality coverage

---

## 2025-11-24

### Added
- **CLAUDE.md** - Project instructions for auto-loading context
  - Triggers automatic reading of CONTEXT.md
  - Instructions for session end behavior
  - File: `CLAUDE.md`

- **CONTEXT.md** - Full project knowledge
  - Plugin architecture and structure
  - Command documentation
  - Hook system explanation
  - Development guidelines
  - File: `CONTEXT.md`

- **CHANGELOG.md** - This file
  - Version history
  - Development progress
  - File: `CHANGELOG.md`

### Changed
- Plugin now uses CLAUDE.md as trigger mechanism for auto-loading
- Documented that hooks work via prompts, not executable scripts

---

## 1.1.0 - 2025-11-24

### Added
- **SessionStart hook** - Auto-loads CONTEXT.md at conversation start
  - Silently reads project context
  - No user interruption
  - File: `hooks/session-start.md`

- **SessionEnd hook** - Auto-updates CHANGELOG.md when conversation ends
  - Logs files changed
  - Records features/decisions
  - File: `hooks/session-end.md`

- **PreCompact hook** - Saves context before auto-compaction
  - Critical for surviving compaction
  - Writes to files before messages removed
  - File: `hooks/pre-compact.md`

- **hooks/hooks.json** - Hook configuration
  - Enables all three hooks
  - File: `hooks/hooks.json`

### Changed
- Version bump to 1.1.0

---

## 1.0.0 - 2025-11-24

### Added
- **Initial plugin release**
  - Forked from minimal-claude by @KenKaiii
  - Renamed to context-keeper
  - New plugin identity and branding

- **`/setup-context` command**
  - Initializes context tracking for any project
  - Creates CONTEXT.md, CHANGELOG.md, config
  - Analyzes project structure automatically
  - File: `commands/setup-context.md`

- **`/show-context` command**
  - Displays current project context summary
  - Shows token usage stats
  - File: `commands/show-context.md`

- **`/update-context` command**
  - Manually update CONTEXT.md
  - Add decisions, patterns, state changes
  - File: `commands/update-context.md`

- **Plugin manifest**
  - Name: context-keeper
  - Author: aaronb458
  - File: `.claude-plugin/plugin.json`

- **Marketplace configuration**
  - File: `.claude-plugin/marketplace.json`

- **README.md** - Comprehensive documentation
  - Installation instructions
  - Usage examples
  - Configuration options
  - Troubleshooting guide

### Removed
- Old minimal-claude commands (setup-code-quality, setup-commits, setup-updates)
- Code quality focused functionality

### Decisions
- **Forked minimal-claude** instead of starting fresh
  - Reason: Proven plugin structure
  - Trade-off: Some cleanup needed

- **Focus on context management** instead of code quality
  - Reason: Solves bigger problem (Claude forgetting context)
  - Trade-off: Lost code quality features (can add back later)

---

## Project Origin

### Based on minimal-claude
- Original author: @KenKaiii
- Original repo: https://github.com/KenKaiii/minimal-claude
- Forked: 2025-11-24

### Why Fork?
- minimal-claude provided excellent plugin structure
- Wanted to add context management features
- Different focus (context vs code quality)

---

*This changelog is maintained by Context Keeper.*
*Last auto-update: 2025-11-24*
