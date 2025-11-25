---
name: update-context
description: Manually update CONTEXT.md with new information or changes
---

Manually update the CONTEXT.md file with new patterns, decisions, or project changes.

## Step 1: Read Current CONTEXT.md

```bash
cat CONTEXT.md
```

If file doesn't exist:
- Tell user: "CONTEXT.md doesn't exist. Run `/setup-context` first to initialize context tracking."
- Exit

## Step 2: Ask User What to Update

Ask: "What would you like to update in CONTEXT.md?"

Options:
1. Add a new decision or pattern
2. Update current state (completed/in-progress/upcoming)
3. Add new important files or sections
4. Update project overview
5. Add notes for Claude
6. Let me analyze recent changes and suggest updates

## Step 3: Perform Update

Based on user choice:

### Option 1: Add Decision/Pattern
- Ask: "What decision or pattern should I document?"
- Add to appropriate section with date
- Format:
  ```markdown
  ### [Pattern/Decision Name] ([Date])
  **Decision:** [What was chosen]
  **Reason:** [Why]
  **Trade-off:** [What was given up]
  ```

### Option 2: Update Current State
- Move items from in-progress → completed
- Add new in-progress items
- Update upcoming items
- Keep concise (3-5 bullets each)

### Option 3: Add Files/Sections
- Ask user which files to document
- Add to "Important Files" section
- Include brief description of each

### Option 4: Update Overview
- Ask what changed about the project
- Update purpose, tech stack, or scope
- Keep overview section to 1-2 paragraphs

### Option 5: Add Claude Notes
- Ask: "What should Claude remember when working on this project?"
- Add to "Notes for Claude" section
- Format as actionable guidelines

### Option 6: Auto-Analyze
- Check git log for recent commits
- Detect new files or significant changes
- Suggest updates based on detected changes
- Ask user to confirm before applying

## Step 4: Update Timestamp

Update the "Last Updated" date at top of CONTEXT.md

## Step 5: Verify Token Count

Estimate token count of updated CONTEXT.md:
- Rough estimate: word_count × 1.3
- Warn if > 3,000 tokens
- If over limit, suggest: "CONTEXT.md is getting large ([X] tokens). Consider:
  1. Moving detailed info to separate docs
  2. Condensing older sections
  3. Creating a CONTEXT_FULL.md with abbreviated CONTEXT.md"

## Step 6: Display Changes

Show user what changed:

```diff
## Updated CONTEXT.md

+ [New content added]
~ [Modified content]

Token count: ~[X] tokens (was ~[Y])
```

Done!
