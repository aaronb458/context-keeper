---
name: pre-compact
description: Save context before Claude's auto-compaction
hook: PreCompact
---

## Save Context Before Compaction

This hook automatically saves your work to CHANGELOG.md BEFORE Claude's auto-compaction removes conversation history.

**This is the critical feature that makes context survive compaction!**

### Step 1: Check for Context Tracking

```bash
ls CHANGELOG.md .claude/context-config.json 2>/dev/null
```

If files don't exist:
- Exit silently (context tracking not set up)

If `pre_compact_save` is false in config:
- Exit silently (user disabled pre-compact saves)

### Step 2: Urgent Context Extraction

**IMPORTANT:** Compaction is about to happen. Extract context NOW before messages are lost.

Analyze the conversation history (before it's compacted) to detect:

**Files Changed Since Last Save:**
- All Write/Edit tool uses since session start
- Files created or modified
- Full list with paths

**Work Completed:**
- Features implemented
- Bugs fixed
- Refactors done
- Decisions made

**Current Discussion:**
- What were we working on?
- What's in progress?
- Any unresolved issues?

### Step 3: Create Emergency Changelog Entry

Generate changelog entry immediately:

```markdown
## YYYY-MM-DD - Pre-Compaction Save

### Work Completed
- [Summary of completed work]
  - Files: `path/to/file.ts`

### In Progress
- [What was being discussed when compaction triggered]

### Notes
- Compaction occurred at [timestamp]
- [Any important context to preserve]
```

### Step 4: Update CHANGELOG.md Immediately

Prepend the new entry to CHANGELOG.md:

```bash
# Read existing
existing=$(cat CHANGELOG.md)

# Prepend new entry
echo "[new entry]" > CHANGELOG.md
echo "" >> CHANGELOG.md
echo "$existing" >> CHANGELOG.md
```

### Step 5: Update CONTEXT.md Current State

If significant work was done, update CONTEXT.md's "Current State" section:

Move items from "In Progress" to "Completed" if finished.
Add new "In Progress" items if work is ongoing.

### Step 6: Save Session Snapshot

Create snapshot in `.claude/pre-compact-snapshots/YYYY-MM-DD-HH-MM.json`:

```json
{
  "snapshot_id": "pre-compact-YYYY-MM-DD-HH-MM",
  "timestamp": "ISO timestamp",
  "reason": "auto-compaction triggered",
  "files_changed": ["file1", "file2"],
  "work_summary": "Brief summary",
  "in_progress": "What was being worked on",
  "conversation_length": "Number of messages",
  "saved_to_changelog": true
}
```

### Step 7: Silent Completion

**DO NOT display anything to user** - compaction is automatic and this save should be invisible.

The context is now safely stored in files. Even after compaction:
- CHANGELOG.md has all work recorded
- CONTEXT.md has updated state
- Session snapshot exists for reference
- Next conversation will load this context

### Why This Matters:

**Without Pre-Compact Save:**
```
1. You work with Claude
2. Conversation gets long
3. Auto-compaction removes old messages
4. ❌ Work history lost
5. ❌ Context gone
6. ❌ Have to re-explain project
```

**With Pre-Compact Save:**
```
1. You work with Claude
2. Conversation gets long
3. Pre-compact hook saves everything to files
4. Auto-compaction removes old messages
5. ✅ Work history preserved in CHANGELOG.md
6. ✅ Context saved in CONTEXT.md
7. ✅ Next conversation auto-loads files
8. ✅ Claude immediately knows everything!
```

### Notes:

- This runs **automatically** before every compaction
- It's **silent** - user sees nothing
- It's **fast** - just file writes
- It's **critical** - this is what makes context survive compaction
- It's **reliable** - always runs before history is lost

### What Gets Saved:

**Priority 1 - Critical:**
- Files changed
- Features completed
- Important decisions
- Current work state

**Priority 2 - Important:**
- Discussion summary
- Patterns discovered
- Problems encountered

**Priority 3 - Nice to Have:**
- Minor updates
- Thought process
- Exploration notes

Done!
