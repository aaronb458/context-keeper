---
name: search-errors
description: Search logged errors for solutions to current problem
---

Search the error log for previously encountered errors and their solutions.

## What This Does

Searches `ERRORS.md` for matching errors and displays solutions. Never solve the same problem twice!

## Step 1: Check for Errors File

```bash
ls ERRORS.md 2>/dev/null
```

If doesn't exist:
- Tell user: "No errors logged yet. Use `/log-error` after fixing errors to build your knowledge base."
- Exit

## Step 2: Get Search Query

If user provided search term:
```
/search-errors "cannot read property"
```
Use that as the search query.

If no search term, ask:
"What error are you encountering? Paste the error message or describe it."

## Step 3: Search ERRORS.md

Search for matches in:
1. Error messages
2. Keywords
3. Solution descriptions
4. Category names
5. File paths

Use fuzzy matching - partial matches count.

## Step 4: Display Results

### If matches found:

```
🔍 Found [N] matching error(s):

---

## [Category]: [Title]
**Date:** YYYY-MM-DD

**Error:**
```
[Error message snippet]
```

**Solution:**
[Solution summary]

**Files:** `file.ts:line`

---

[Additional matches...]

---

Does one of these match your issue?
- If yes: Follow the solution above
- If no: Describe your error and I'll help solve it, then we can log it
```

### If no matches:

```
🔍 No matching errors found for "[query]"

This might be a new error! Let me help you solve it.

Paste the full error message and I'll:
1. Help debug and fix it
2. Log the solution with `/log-error` for next time
```

## Step 5: Offer to Help

If no match or user needs more help:
- Offer to debug the current error
- Remind to log solution when fixed: `/log-error`

## Search Examples

### Example 1: Direct Search
```
/search-errors ENOENT

→ Searches for "ENOENT" in all error entries
→ Finds: "File not found: ENOENT"
→ Shows solution: "File path was relative, changed to absolute with path.resolve()"
```

### Example 2: Paste Error Message
```
/search-errors "TypeError: Cannot read property 'map' of undefined"

→ Searches for matching TypeError
→ Finds: Previous null check solution
→ Shows: "Add null coalescing: data?.items ?? []"
```

### Example 3: Category Search
```
/search-errors dependency

→ Shows all Dependency Error category entries
→ Lists common npm/package issues and solutions
```

### Example 4: No Args
```
/search-errors

→ Prompts: "What error are you encountering?"
→ User pastes error
→ Searches and displays results
```

## Auto-Search

When Claude detects an error in conversation (from Bash output, error messages, etc.):

1. Automatically search ERRORS.md
2. If match found, display:
   ```
   💡 I found a previous solution for this error!

   [Solution from ERRORS.md]

   Try this fix - it worked before on YYYY-MM-DD.
   ```
3. If no match, help solve then offer to log

## Statistics Display

```
/search-errors --stats
```

Shows:
```
📊 Error Log Statistics

Total Errors Logged: 23
Categories:
  - Runtime Error: 8
  - Type Error: 6
  - Build Error: 4
  - Dependency Error: 3
  - Other: 2

Most Common Keywords: map, undefined, null, import, module

Last Error Logged: 2025-11-24
```

Done!
