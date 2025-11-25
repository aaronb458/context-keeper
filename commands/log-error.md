---
name: log-error
description: Log an error and its solution for future reference
---

Log an error you encountered and how you fixed it, so you never have to solve it twice.

## What This Does

Creates a searchable error database that tracks:
- **Error message** - The actual error
- **Cause** - What caused it
- **Solution** - How it was fixed
- **Files affected** - Where to look

Errors are stored in `ERRORS.md` for future reference.

## Step 1: Check for Errors File

```bash
ls ERRORS.md 2>/dev/null
```

If doesn't exist, create it with header:

```markdown
# Error Solutions Log

This file tracks errors encountered and their solutions. Search here before debugging!

---

```

## Step 2: Gather Error Information

Ask the user (or extract from recent conversation):

**Required:**
1. **Error Message**: The actual error text (can be partial)
2. **Solution**: How was it fixed?

**Auto-detected (if possible):**
3. **Cause**: What caused the error?
4. **Files**: Which files were involved?
5. **Category**: Type of error (build, runtime, type, dependency, etc.)

## Step 3: Generate Error Entry

```markdown
## [Category]: [Short Description]

**Date:** YYYY-MM-DD
**Error:**
```
[Actual error message]
```

**Cause:**
[What caused this error]

**Solution:**
[Step-by-step fix]

**Files:**
- `path/to/file.ts:line` - [What was changed]

**Keywords:** [searchable terms]

---
```

## Step 4: Categorize Error

Detect category from error message:

- **Build Error** - Compilation, bundling failures
- **Type Error** - TypeScript, type mismatches
- **Runtime Error** - Crashes, exceptions at runtime
- **Dependency Error** - Package issues, version conflicts
- **Config Error** - Configuration problems
- **API Error** - HTTP, external service errors
- **Database Error** - Query, connection issues
- **Auth Error** - Authentication, permissions
- **Other** - Uncategorized

## Step 5: Extract Keywords

Generate searchable keywords from:
- Error message text
- File names
- Package names
- Error codes

Example: `["ENOENT", "file not found", "fs", "readFile"]`

## Step 6: Append to ERRORS.md

Add the new error entry to the file.

## Step 7: Confirm to User

Display:

```
✅ Error logged!

## [Category]: [Short Description]
**Solution:** [One-line summary]
**Keywords:** [keyword1, keyword2, keyword3]

Saved to: ERRORS.md

Next time this happens, I'll remember the solution!
```

## Quick Mode

If user describes error inline:

```
/log-error "Cannot read property 'map' of undefined" - fixed by adding null check
```

**Extract:**
- Error: Cannot read property 'map' of undefined
- Solution: Added null check
- Category: Runtime Error
- Keywords: map, undefined, null check, TypeError

## Auto-Detection

When you help fix an error in conversation, prompt:

"Error fixed! Would you like me to log this for future reference? `/log-error`"

## Search Integration

When user encounters an error, automatically check ERRORS.md:

1. Extract key terms from error message
2. Search ERRORS.md for matches
3. If found: "I found a previous solution for this error!"
4. Display the solution

## Examples

### Example 1: Type Error
```
/log-error

Error: Type 'string' is not assignable to type 'number'
Cause: API returns string but interface expects number
Solution: Added parseInt() to convert string to number
Files: lib/api.ts:45
```

### Example 2: Dependency Error
```
/log-error "Module not found: Can't resolve 'lodash'"

→ Auto-generates:
Category: Dependency Error
Error: Module not found: Can't resolve 'lodash'
Solution: npm install lodash
Keywords: module not found, lodash, dependency, npm
```

### Example 3: Build Error
```
/log-error

The build was failing with "heap out of memory". Fixed by adding
NODE_OPTIONS="--max-old-space-size=4096" to the build script.

→ Auto-extracts and generates full entry
```

## Error Entry Format Details

### Full Format:
```markdown
## Runtime Error: Array map on undefined

**Date:** 2025-11-24
**Error:**
```
TypeError: Cannot read property 'map' of undefined
    at UserList (components/UserList.tsx:15:23)
    at renderWithHooks (react-dom.development.js:14985:18)
```

**Cause:**
API returned null instead of empty array when no users exist.

**Solution:**
1. Added null check before mapping:
   ```typescript
   const users = data?.users ?? [];
   return users.map(user => <UserCard key={user.id} user={user} />);
   ```
2. Also updated API to always return empty array instead of null

**Files:**
- `components/UserList.tsx:15` - Added null coalescing
- `pages/api/users.ts:28` - Changed null to []

**Keywords:** map, undefined, null, TypeError, array, API response

---
```

## Notes

- Include the actual error message - makes searching easier
- Document the root cause, not just the fix
- Add file paths and line numbers when known
- Keep solutions actionable - someone should be able to follow them
- Keywords help with searching - add variations

Done!
