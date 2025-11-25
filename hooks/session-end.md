---
name: session-end
description: Auto-update CHANGELOG.md when conversation ends
hook: SessionEnd
---

## Auto-Update Project Changelog

This hook automatically updates CHANGELOG.md when a conversation ends, logging your work for future reference.

### Step 1: Check for Context Tracking

```bash
ls CHANGELOG.md .claude/context-config.json 2>/dev/null
```

If files don't exist:
- Exit silently (context tracking not set up)

If `auto_log_changes` is false in config:
- Exit silently (user disabled auto-logging)

### Step 2: Analyze Session Work

Review the conversation history to detect:

**Files Changed:**
- Check conversation for Write, Edit tool uses
- List all files that were created or modified
- Example: `["lib/auth.ts", "middleware/auth.ts", "app/api/auth/route.ts"]`

**Features/Changes:**
- Detect what was built or modified
- Look for patterns like:
  - "Added [feature]"
  - "Implemented [functionality]"
  - "Fixed [bug]"
  - "Refactored [component]"
  - "Updated [system]"

**Decisions Made:**
- Look for decision keywords:
  - "decided to use"
  - "chose [X] over [Y]"
  - "going with [approach]"
  - "opted for"
- Extract reasoning if discussed

**Min Threshold:**
- Only log if `min_files_changed` threshold met (default: 1)
- Only log if conversation length > `min_conversation_length` (default: 10 messages)

### Step 3: Skip if No Significant Work

If no files were changed AND no significant discussion:
- Exit silently (nothing to log)

If only trivial changes (typos, formatting):
- Exit silently

### Step 4: Generate Changelog Entry

Create a changelog entry for today's date in Keep-a-Changelog format:

```markdown
## YYYY-MM-DD

### Added
- [Feature/component added]
  - [Implementation details]
  - Files: `path/to/file.ts`, `path/to/other.ts`

### Changed
- [What was modified and why]
  - Files: `path/to/file.ts`

### Fixed
- [Bug description and solution]
  - Files: `path/to/file.ts`

### Decisions
- **Choice:** [What was chosen]
  - **Reason:** [Why it was chosen]
  - **Trade-off:** [What was given up]
```

### Step 5: Update CHANGELOG.md

Read existing CHANGELOG.md:

```bash
cat CHANGELOG.md
```

Check if there's already an entry for today's date:
- If yes: Append to existing entry (merge sections)
- If no: Insert new entry after `## [Unreleased]` section

Update the file with new content.

### Step 6: Update "Last Updated" in CONTEXT.md

If CONTEXT.md exists and changes were significant:

Update the timestamp at the top:
```markdown
> **Last Updated:** YYYY-MM-DD HH:MM
```

### Step 7: Show Quiet Confirmation

Display a subtle confirmation to user:

```
✓ Context updated with [N] changes
```

Where [N] is:
- Number of files changed, or
- Number of features added, or
- "session work"

Keep it minimal - one line only.

### Step 8: Save Session Log (Optional)

If `session_tracking.enabled` is true in config:

Create session log in `.claude/session-logs/YYYY-MM-DD-NNN.json`:

```json
{
  "session_id": "YYYY-MM-DD-NNN",
  "start_time": "ISO timestamp",
  "end_time": "ISO timestamp",
  "files_changed": ["file1", "file2"],
  "summary": "Brief summary of work done",
  "changelog_entry_added": true
}
```

### Notes:

- This runs **automatically** when conversation ends (user types `exit` or closes)
- It's **smart** - only logs significant work
- It's **quiet** - just one line of confirmation
- It's **persistent** - survives restarts and compactions
- It's **cumulative** - builds project history over time

### What Gets Logged:

**Always Log:**
- New features added
- Bugs fixed
- Major refactors
- Important decisions
- System changes

**Sometimes Log:**
- Minor updates if part of larger change
- Documentation if substantial
- Configuration changes if significant

**Never Log:**
- Typo fixes alone
- Minor formatting changes
- Exploratory work with no output
- Conversations with no file changes

Done!
